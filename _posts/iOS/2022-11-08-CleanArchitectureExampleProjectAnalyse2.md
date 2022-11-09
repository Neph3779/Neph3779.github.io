---
layout: post
title: "Clean Architecture 예제 프로젝트 분석 2 - UseCase & Repository"
categories: [iOS]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## UseCase

### SearchMoviesUseCase

```swift
protocol SearchMoviesUseCase {
    func execute(requestValue: SearchMoviesUseCaseRequestValue,
                 cached: @escaping (MoviesPage) -> Void,
                 completion: @escaping (Result<MoviesPage, Error>) -> Void) -> Cancellable?
}

final class DefaultSearchMoviesUseCase: SearchMoviesUseCase {

    private let moviesRepository: MoviesRepository
    private let moviesQueriesRepository: MoviesQueriesRepository

    init(moviesRepository: MoviesRepository, // 영화 정보 자체를 저장하는 객체
         moviesQueriesRepository: MoviesQueriesRepository) { // 영화 검색 관련 내용들을 보관하는 객체

        self.moviesRepository = moviesRepository
        self.moviesQueriesRepository = moviesQueriesRepository
    }

    func execute(requestValue: SearchMoviesUseCaseRequestValue,
                 cached: @escaping (MoviesPage) -> Void,
                 completion: @escaping (Result<MoviesPage, Error>) -> Void) -> Cancellable? {

        return moviesRepository.fetchMoviesList(query: requestValue.query,
                                                page: requestValue.page,
                                                cached: cached,
                                                completion: { result in

            if case .success = result {
                self.moviesQueriesRepository.saveRecentQuery(query: requestValue.query) { _ in }
            }

            completion(result)
        })
    }
}
```



Presentation Layer의 ViewModel이 Domain Layer의 UseCase를 호출

UseCase는 Data Layer인 repository로부터 데이터를 얻어오게 되는데 

이때 의존성이 Domain -> Data가 되는 것을 방지하기 위해 Domain이 사용할 인터페이스를 프로토콜로 만들어서 정의해둠. 

UseCase는 이 프로토콜을 사용.

즉, Data Layer에 변경사항이 생기는 경우 사용중이던 인터페이스를 건드려서는 안됨



### FetchRecentMovieQueriesUseCase

```swift
final class FetchRecentMovieQueriesUseCase: UseCase {

    struct RequestValue {
        let maxCount: Int
    }
    typealias ResultValue = (Result<[MovieQuery], Error>)

    private let requestValue: RequestValue
    private let completion: (ResultValue) -> Void
    private let moviesQueriesRepository: MoviesQueriesRepository

    init(requestValue: RequestValue,
         completion: @escaping (ResultValue) -> Void,
         moviesQueriesRepository: MoviesQueriesRepository) {

        self.requestValue = requestValue
        self.completion = completion
        self.moviesQueriesRepository = moviesQueriesRepository
    }
    
    func start() -> Cancellable? {

        moviesQueriesRepository.fetchRecentsQueries(maxCount: requestValue.maxCount, completion: completion)
        return nil
    }
}
```

UseCase 프로토콜을 일관되게 채택하도록 하여 사용할수도 있음

위의 SearchMoviesUseCase 예제에서는 프로토콜로 정의된 execute 메서드에 대한 인터페이스가 맞춤형이지만

UseCase 프로토콜을 채택할 시 인자를 받지 않는 start 메서드를 사용하여 작업을 진행



### 두 방법의 장단점

우선 두 경우 모두 프로토콜을 사용하므로 Mocking이 가능함

전자는 모든 UseCase별로 맞춤 프로토콜을 제작하여야 하고

후자는 같은 인터페이스의 start() 메서드를 사용하므로 일관성 측면에서 이점이 있음

하지만 인자를 받지 않는다는 전제가 고정이므로 상황에 유하게 대처하기 어려울 수도 있음

개인적으로는 전자의 방법을 사용하는 것이 더 좋아보임



## Repository

Repository는 Data Layer에 소속되어있음

Use Case는 Domain Layer에 소속되어있지만 Repository를 가져다 써야함

inner 요소가 outer 요소에 의존성을 가지게 되므로 이를 "인터페이스"를 규정함으로써 해결함

Domain Layer에서 인터페이스를 정해주면 그에 맞게 Repository는 필요한 메서드를 만들어줘야함

```swift
protocol MoviesRepository {
    @discardableResult
    func fetchMoviesList(query: MovieQuery, page: Int,
                         cached: @escaping (MoviesPage) -> Void,
                         completion: @escaping (Result<MoviesPage, Error>) -> Void) -> Cancellable?
}
```

```swift
protocol MoviesQueriesRepository {
    func fetchRecentsQueries(maxCount: Int, completion: @escaping (Result<[MovieQuery], Error>) -> Void)
    func saveRecentQuery(query: MovieQuery, completion: @escaping (Result<MovieQuery, Error>) -> Void)
}
```

이렇게 Interface들을 정의해두고 사용하는 것.

기획이 달라지는 경우 (아예 새로운 기능이 추가되는 경우)가 아니라면

이렇듯 네트워킹 로직의 변경, 화면을 구성하는 코드의 변경 등은 Domain Layer에 영향을 주지 않게 됨



```swift
final class DefaultMoviesRepository {

    private let dataTransferService: DataTransferService
    private let cache: MoviesResponseStorage

    init(dataTransferService: DataTransferService, cache: MoviesResponseStorage) {
        self.dataTransferService = dataTransferService
        self.cache = cache
    }
}

extension DefaultMoviesRepository: MoviesRepository {

    public func fetchMoviesList(query: MovieQuery, page: Int,
                                cached: @escaping (MoviesPage) -> Void,
                                completion: @escaping (Result<MoviesPage, Error>) -> Void) -> Cancellable? {

        let requestDTO = MoviesRequestDTO(query: query.query, page: page)
        let task = RepositoryTask()

        cache.getResponse(for: requestDTO) { result in

            if case let .success(responseDTO?) = result {
                cached(responseDTO.toDomain())
            }
            guard !task.isCancelled else { return }

            let endpoint = APIEndpoints.getMovies(with: requestDTO)
            task.networkTask = self.dataTransferService.request(with: endpoint) { result in
                switch result {
                case .success(let responseDTO):
                    self.cache.save(response: responseDTO, for: requestDTO)
                    completion(.success(responseDTO.toDomain()))
                case .failure(let error):
                    completion(.failure(error))
                }
            }
        }
        return task
    }
}
```

실제 Repository의 구현부는 Data Layer에 있으며 인터페이스에 맞게 이곳에서 메서드를 제작함