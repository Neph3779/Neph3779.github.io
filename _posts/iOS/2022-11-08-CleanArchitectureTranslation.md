---
layout: post
title: "[번역] Clean Architecture and MVVM on iOS"
categories: [iOS]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

본 글은 개인 학습목적으로 [해당글](https://tech.olx.com/clean-architecture-and-mvvm-on-ios-c9d167d9f5b3)을 번역한 글입니다. (의역 다수 존재, 삭제된 부분 존재)

<br/> 

## 개요

개발을 할때 디자인 패턴만큼이나 중요한 것이 아키텍쳐 패턴(architectural patterns)입니다. 

개발 분야에는 다양한 아키텍쳐 패턴들이 존재하며 모바일 소프트웨어 엔지니어링에서는 MVVM, 클린 아키텍쳐, Redux 패턴이 가장 널리 사용되고 있습니다.

이 글에서는 [예시 프로젝트](https://github.com/kudoleh/iOS-Clean-Architecture-MVVM)를 통해 MVVM패턴과 클린 아키텍쳐 패턴을 iOS 앱에 적용한 예제 프로젝트를 소개할 것입니다.



## CleanArchitecture의 구성요소

![img](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221107193339.png)

위의 클린 아키텍쳐를 나타낸 그래프에서 볼 수 있듯 앱에는 서로 다른 Layer들이 존재합니다. 

이 아키텍쳐에서 가장 중요한건 내부 Layer가 외부 Layer에 의존성을 가지지 않아야 한다는 것입니다. 

밖으로부터 안쪽으로 그려져 있는 화살표는 의존성의 방향을 나타낸 것입니다. 

화살표의 방향에서 알 수 있듯 의존성은 외부 Layer에서 내부 Layer로의 의존성만 존재합니다.

모든 Layer를 그룹화 하면 Presentation Layer, Domain Layer, Data Layer로 나눌 수 있습니다.

### 3가지 Layer

![img](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221107193333.png)

**Domain Layer**

도메인 Layer(비즈니스 로직)는 양파의 가장 안쪽 부분에 해당합니다. (다른 Layer로의 의존성이 없으며 완전히 고립되어 있음) 

도메인 Layer는 Entities(비즈니스 모델들), Use Cases, Repository Interfaces를 가지고 있습니다. 

도메인 Layer는 다른 프로젝트에서 재사용될 수 있으며, 도메인 Layer의 분리를 통해 유닛 테스트 시 host app이 필요하지 않도록 만들어줍니다. 

이는 써드파티 라이브러리를 포함한 그 어떠한 의존성이 없기 때문에 가능한 것이며, UseCase의 테스트를 아주 빠르게 할 수 있습니다.

<br/> 

> Note: 도메인 Layer는 다른 Layer의 그 어떠한 것도 포함하고 있지 않아야 합니다. 
>
> Presentation Layer가 가지고 있는 UIKit이나 Data Layer가 가지고 있는 Codable이 그 예시입니다.

<br/> 

좋은 아키텍쳐일수록 프레임워크, 툴, 개발환경 등과 무관하게 Use cases의 구조를 온전히 설명할 수 있어야 하기 때문에 

좋은 아키텍쳐일수록 Use Cases에 집중되어있는 모습을 보입니다.

<br/> 

**Presentation Layer**

Presentation Layer에는 UI(UIViewController)가 포함되어 있습니다.

View는 하나 이상의 Use Case들을 실행하는 ViewModel(Presenter)들과 함께 조직화되어있습니다.

Presentation Layer는 오직 도메인 Layer에만 의존성을 가집니다.

<br/> 

**Data Layer**

Data Layer는 레포지토리 구현부와 하나 이상의 데이터소스를 가집니다. (레포지토리 인터페이스는 도메인 Layer에 있음)

레포지토리는 다른 데이터 소스로부터 데이터를 조직화할 책임을 지니고 있습니다.

이때 데이터 소스는 리모트(원격 저장소의 데이터; 서버 DB)와 로컬(앱 내부 저장소의 데이터; 로컬 DB)이 존재합니다. 

데이터 Layer는 오직 도메인 Layer에만 의존성을 가집니다. 

데이터 Layer에는 네트워크로부터 받아온 JSON 데이터를 도메인 모델에 mapping하는 작업도 존재할 수 있습니다. (e.g. Decodable을 채택한 모델)

![img](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221107195817.png)

위의 그림에는 각각의 요소들이 의존하는 방향과 데이터의 흐름(요청/응답)이 나타나 있습니다.

데이터 레포지토리 인터페이스를 사용할때 의존성 역전이 일어나는 것을 확인할 수 있습니다.

각 Layer들의 설명은 글의 시작부에 언급된 예시 프로젝트에 존재합니다.

<br/> 

**Data Flow**

1. View(UI)가 ViewModel(Presenter)로부터 메서드를 호출
2. ViewModel은 Use Case를 실행
3. Use Case는 유저로부터 얻은 데이터와 레포지토리로부터 얻은 데이터를 결합 (combine)
4. 각 레포지토리는 리모트 데이터(네트워크)와 Persistent DB, In-memory 데이터로부터 온 데이터를 반환
5. 정보가 View(UI)로 다시 흘러가고 이를 화면에 띄움

<br/> 

**Dependency Direction**

Presentation Layer → Domain Layer ← Data Repository Layer

<br/> 

**Presentation Layer (MVVM)** = ViewModels(Presenters) + Views(UI)

**Domain Layer** = Entities + Use cases + Repositories Interfaces

**Data Repositories Layer** = Repositories Implementations + API(Network) + Persistence DB

<br/>

## 예제 프로젝트: "Movies App"

![img](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221107221414.png)



### 도메인 Layer

예제 프로젝트 내의 폴더를 보면 도메인 Layer를 찾을 수 있습니다. 

도메인에는 Entities와 영화를 검색하고 성공적으로 작업을 마친 쿼리를 저장하는 SearchMoviesUseCase와 

의존성 역전 문제 해결을 위해 필요한 Data Repositories Interfaces가 존재합니다.

```swift
protocol SearchMoviesUseCase {
    func execute(requestValue: SearchMoviesUseCaseRequestValue,
                 completion: @escaping (Result<MoviesPage, Error>) -> Void) -> Cancellable?
}

final class DefaultSearchMoviesUseCase: SearchMoviesUseCase {

    private let moviesRepository: MoviesRepository
    private let moviesQueriesRepository: MoviesQueriesRepository
    
    init(moviesRepository: MoviesRepository, moviesQueriesRepository: MoviesQueriesRepository) {
        self.moviesRepository = moviesRepository
        self.moviesQueriesRepository = moviesQueriesRepository
    }
    
    func execute(requestValue: SearchMoviesUseCaseRequestValue,
                 completion: @escaping (Result<MoviesPage, Error>) -> Void) -> Cancellable? {
        return moviesRepository.fetchMoviesList(query: requestValue.query, page: requestValue.page) { result in
            
            if case .success = result {
                self.moviesQueriesRepository.saveRecentQuery(query: requestValue.query) { _ in }
            }

            completion(result)
        }
    }
}

// Repository Interfaces
protocol MoviesRepository {
    func fetchMoviesList(query: MovieQuery, page: Int, completion: @escaping (Result<MoviesPage, Error>) -> Void) -> Cancellable?
}

protocol MoviesQueriesRepository {
    func fetchRecentsQueries(maxCount: Int, completion: @escaping (Result<[MovieQuery], Error>) -> Void)
    func saveRecentQuery(query: MovieQuery, completion: @escaping (Result<MovieQuery, Error>) -> Void)
}
```

<br/> 

> Note: Use Case를 만드는 또 다른 방법은 *UseCase*의 start() 메서드를 통해 생성하고
>
> 모든 use case들이 UseCase 프로토콜을 채택하도록 하는 것입니다. 
>
> 예제 프로젝트에서는 [FetchRecentMovieQueriesUseCase](https://github.com/kudoleh/iOS-Clean-Architecture-MVVM/blob/master/ExampleMVVM/Domain/UseCases/FetchRecentMovieQueriesUseCase.swift)가 이 방식을 사용하고 있습니다. 
>
> 추가로 Use case는 Interactor라고도 불립니다.

> Note: *UseCase*는 다른 *UseCases*에 종속적일 수 있습니다.

<br/> 

### Presentation Layer

Presentation Layer에는 MoviesListViewModel과 이의 item들을 observe하는 MoviesListView가 포함되어 있습니다. 

MoviesListViewModel은 UIKit을 import하지 않는데, 이는 ViewModel을  UIKit과 같은 UI 프레임워크로부터 격리하여 재사용과 리팩토링이 쉬워지도록 하기 위함입니다.

View를 구성하는 코드가 변경되더라도(UIKit 코드의 SwiftUI로의 리팩토링 등) ViewModel은 바뀌지 않아도 되므로 리팩토링이 쉬워집니다.

<br/> 

```swift
// Note: We cannot have any UI frameworks(like UIKit or SwiftUI) imports here. 
protocol MoviesListViewModelInput {
    func didSearch(query: String)
    func didSelect(at indexPath: IndexPath)
}

protocol MoviesListViewModelOutput {
    var items: Observable<[MoviesListItemViewModel]> { get }
    var error: Observable<String> { get }
}

protocol MoviesListViewModel: MoviesListViewModelInput, MoviesListViewModelOutput { }

struct MoviesListViewModelActions {
    // Note: if you would need to edit movie inside Details screen and update this 
    // MoviesList screen with Updated movie then you would need this closure:
    //  showMovieDetails: (Movie, @escaping (_ updated: Movie) -> Void) -> Void
    let showMovieDetails: (Movie) -> Void
}

final class DefaultMoviesListViewModel: MoviesListViewModel {
    
    private let searchMoviesUseCase: SearchMoviesUseCase
    private let actions: MoviesListViewModelActions?
    
    private var movies: [Movie] = []
    
    // MARK: - OUTPUT
    let items: Observable<[MoviesListItemViewModel]> = Observable([])
    let error: Observable<String> = Observable("")
    
    init(searchMoviesUseCase: SearchMoviesUseCase,
         actions: MoviesListViewModelActions) {
        self.searchMoviesUseCase = searchMoviesUseCase
        self.actions = actions
    }
    
    private func load(movieQuery: MovieQuery) {
        
        searchMoviesUseCase.execute(movieQuery: movieQuery) { result in
            switch result {
            case .success(let moviesPage):
                // Note: We must map here from Domain Entities into Item View Models. Separation of Domain and View
                self.items.value += moviesPage.movies.map(MoviesListItemViewModel.init)
                self.movies += moviesPage.movies
            case .failure:
                self.error.value = NSLocalizedString("Failed loading movies", comment: "")
            }
        }
    }
}

// MARK: - INPUT. View event methods
extension MoviesListViewModel {
    
    func didSearch(query: String) {
        load(movieQuery: MovieQuery(query: query))
    }
    
    func didSelect(at indexPath: IndexPath) {
        actions?.showMovieDetails(movies[indexPath.row])
    }
}

// Note: This item view model is to display data and does not contain any domain model to prevent views accessing it
struct MoviesListItemViewModel: Equatable {
    let title: String
}

extension MoviesListItemViewModel {
    init(movie: Movie) {
        self.title = movie.title ?? ""
    }
}
```


<br/> 

> Note: MoviesListViewModelInput과 MoviesListViewModelOutput이라는 인터페이스를 사용함으로써 
>
> ViewModel을 mocking하기 쉽게 만들며 MoviesListViewController를 testable하게 만듭니다. 
>
> 게다가 MoviesListViewModelActions 클로저는 MoviesSearchFlowCoordinator에게 언제 다른 view를 present 시킬지를 알려줍니다. 
>
> action 클로저가 호출되면 coordinator는 movie details screen을 present 시킵니다. 
>
> group action을 위해 struct를 사용하는데 이는 나중에 action을 더 쉽게 추가하기 위해서입니다.

<br/> 

Presentation Layer는 MoviesListViewModel에 바인딩된 MoviesListViewController도 포함하고 있습니다.

UI는 비즈니스 로직이나 어플리케이션 로직(비즈니스 모델과 UseCases)에는 접근할 수 없고, 오직 ViewModel만이 가능합니다. 이것은 seperation of concerns라고 합니다. 비즈니스 모델은 곧바로 View(UI)로 전달할 수 없으므로 비즈니스 모델을 ViewModel에 mapping하여 내부에 집어넣고 ViewModel로 하여금 View에 전달하도록 합니다.

View로부터 영화검색 이벤트 콜을 ViewModel에 전달하여 영화검색이 진행되도록 만들었습니다.

```swift
import UIKit

final class MoviesListViewController: UIViewController, StoryboardInstantiable, UISearchBarDelegate {
    
    private var viewModel: MoviesListViewModel!
    
    final class func create(with viewModel: MoviesListViewModel) -> MoviesListViewController {
        let vc = MoviesListViewController.instantiateViewController()
        vc.viewModel = viewModel
        return vc
    }

    override func viewDidLoad() {
        super.viewDidLoad()

        bind(to: viewModel)
    }
    
    private func bind(to viewModel: MoviesListViewModel) {
        viewModel.items.observe(on: self) { [weak self] items in
            self?.moviesTableViewController?.items = items
        }
        viewModel.error.observe(on: self) { [weak self] error in
            self?.showError(error)
        }
    }
    
    func searchBarSearchButtonClicked(_ searchBar: UISearchBar) {
        guard let searchText = searchBar.text, !searchText.isEmpty else { return }
        viewModel.didSearch(query: searchText)
    }
}
```


<br/> 

> Note: items를 observe하고 변경이 발생하면 view를 reload합니다. 여기서 [Observable](https://github.com/kudoleh/iOS-Clean-Architecture-MVVM/blob/master/ExampleMVVM/Presentation/Utils/Observable.swift)이라는 것을 사용하는데 아래의 MVVM 섹션에서 설명되는 내용입니다.


<br/> 

*showMovieDetails(movie:)* 함수를 MoviesSearchFlowCoordinator 내부에 있는 MoviesListViewModel의 Action으로 할당하여 영화의 detail 화면을 flow coordinator를 통해 present합니다.

```swift
protocol MoviesSearchFlowCoordinatorDependencies  {
    func makeMoviesListViewController(actions: MoviesListViewModelActions) -> MoviesListViewController
    func makeMoviesDetailsViewController(movie: Movie) -> UIViewController
}

final class MoviesSearchFlowCoordinator {
    
    private weak var navigationController: UINavigationController?
    private let dependencies: MoviesSearchFlowCoordinatorDependencies

    private weak var moviesListVC: MoviesListViewController?
    private weak var moviesQueriesSuggestionsVC: UIViewController?

    init(navigationController: UINavigationController,
         dependencies: MoviesSearchFlowCoordinatorDependencies) {
        self.navigationController = navigationController
        self.dependencies = dependencies
    }
    
    func start() {
        // Note: here we keep strong reference with actions, this way this flow do not need to be strong referenced
        let actions = MoviesListViewModelActions(showMovieDetails: showMovieDetails)
        let vc = dependencies.makeMoviesListViewController(actions: actions)

        navigationController?.pushViewController(vc, animated: false)
        moviesListVC = vc
    }

    private func showMovieDetails(movie: Movie) {
        let vc = dependencies.makeMoviesDetailsViewController(movie: movie)
        navigationController?.pushViewController(vc, animated: true)
    }
}
```


<br/> 

> Note: presentation 로직을 위해 Flow Coordinator를 사용하여 ViewController의 사이즈와 책임을 줄입니다. Flow에는 강한참조를 걸어 필요한동안 deallocated 되지 않도록 합니다.

이러한 접근방법을 통해 서로 다른 view들을 같은  ViewModel에 그대로 넣는 것이 가능합니다. iOS 13.0을 지원할 수 있다면 SwiftUI View를 UIKit 대신에 사용하여 같은  ViewModel에 바인딩해볼 수도 있습니다. 예제 프로젝트에는MoviesQueriesSuggestionList에 대한 SwiftUI 사용 예제도 수록되어 있습니다.


<br/> 

### Data Layer

Data Layer는 DefaultMoviesRepository를 포함합니다. Data Layer는 Domain Layer에서 정의된 인터페이스를 따릅니다. (Dependency Inversion) 여기에 json 데이터와 CoreData Entity의 도메인 모델로의 mapping을 추가합니다. (json 데이터의 경우 Decodable 채택이 될 것 입니다.)

```swift
final class DefaultMoviesRepository {
    
    private let dataTransferService: DataTransfer
    
    init(dataTransferService: DataTransfer) {
        self.dataTransferService = dataTransferService
    }
}

extension DefaultMoviesRepository: MoviesRepository {
    
    public func fetchMoviesList(query: MovieQuery, page: Int, completion: @escaping (Result<MoviesPage, Error>) -> Void) -> Cancellable? {
        
        let endpoint = APIEndpoints.getMovies(with: MoviesRequestDTO(query: query.query,
                                                                     page: page))
        return dataTransferService.request(with: endpoint) { (response: Result<MoviesResponseDTO, Error>) in
            switch response {
            case .success(let moviesResponseDTO):
                completion(.success(moviesResponseDTO.toDomain()))
            case .failure(let error):
                completion(.failure(error))
            }
        }
    }
}

// MARK: - Data Transfer Object (DTO)
// It is used as intermediate object to encode/decode JSON response into domain, inside DataTransferService
struct MoviesRequestDTO: Encodable {
    let query: String
    let page: Int
}

struct MoviesResponseDTO: Decodable {
    private enum CodingKeys: String, CodingKey {
        case page
        case totalPages = "total_pages"
        case movies = "results"
    }
    let page: Int
    let totalPages: Int
    let movies: [MovieDTO]
}
...
// MARK: - Mappings to Domain
extension MoviesResponseDTO {
    func toDomain() -> MoviesPage {
        return .init(page: page,
                     totalPages: totalPages,
                     movies: movies.map { $0.toDomain() })
    }
}
...
```


<br/> 

> Note: Data Transfer Objects (DTO)는 json response를 도메인에 매핑하기 위한 중간 매개 객체로 사용됩니다. 또, 엔드포인트 응답을 캐싱하고 싶다면 persistent storage에 persistent object로 매핑하여 저장해야합니다. (e.g. DTO -> NSManagedObject)

<br/> 

일반적으로 Data Repository는 API Data Service와 Persistent Data Storage를 주입받을 수 있습니다. Data Repository는 이 두개의 의존성을 가지고 있습니다. 첫번째 규칙은 persistent  storage에 캐시된 데이터가 있는지 확인하는 것입니다. (NSMangedObject는 DTO 객체를 통해 도메인에 매핑되어있으며, *cached data* 클로저에서 전달되어집니다.) 그 뒤에 API Data Service를 호출해 최신 데이터를 가져옵니다. 그리고나면 Persistent Storage가 이 최신 데이터로 업데이트됩니다. (DTO는 Persistent Object로 매핑되고 저장됩니다.) 그 후 DTO가 도메인에 매핑되고 updated data와 completion 클로저를 전달받습니다. 이러한 방법은 유저가 데이터를 순식간에 확인할 수 있도록 해줍니다. 설령 인터넷에 연결되어있지 않더라도 유저는 캐싱된 데이터를 볼 수 있습니다.

Storage와 API는 완전히 다른 구현으로 대체될 수 있습니다. (CoreData에서 Realm으로 대체하는 경우 등) 이는 앱의 다른 Layer가 이 변화에 영향을 받지 않기 때문입니다. 이는 DB가 이 구조에서 세부적인 요소에 속하기 때문입니다.

<br/> 

### Infrastructure Layer (Network)

네트워크 프레임워크를 래핑하는 Layer입니다. 이는 Alamofire나 다른 프레임워크로 대체될 수 있습니다. 이 Layer는 네트워크 파라미터(base URL 등)로 구성될 수 있으며 엔드포인트를 규정하거나 데이터 매핑 메서드를 포함할수도 있습니다. (Decodable을 채택한 객체를 이용하여)

```swift
struct APIEndpoints {
    
    static func getMovies(with moviesRequestDTO: MoviesRequestDTO) -> Endpoint<MoviesResponseDTO> {

        return Endpoint(path: "search/movie/",
                        method: .get,
                        queryParametersEncodable: moviesRequestDTO)
    }
}


let config = ApiDataNetworkConfig(baseURL: URL(string: appConfigurations.apiBaseURL)!,
                                  queryParameters: ["api_key": appConfigurations.apiKey])
let apiDataNetwork = DefaultNetworkService(session: URLSession.shared,
                                           config: config)

let endpoint = APIEndpoints.getMovies(with: MoviesRequestDTO(query: query.query,
                                                             page: page))
dataTransferService.request(with: endpoint) { (response: Result<MoviesResponseDTO, Error>) in
    let moviesPage = try? response.get()
}
```


<br/> 

### MVVM

Model-View-ViewModel 패턴(MVVM)은 UI와 도메인 사이의 깔끔한 분리를 가능하게 해줍니다.

클린 아키텍쳐와 함께 사용하면  Presentation과 UILayer 사이의 문제도 해결할 수 있습니다.

같은 ViewModel를 다른 view에 사용할 수 있습니다. 예를 들어 CarsAroundListView와 CarsAroundMapView에 CarsAroundViewModel를 공용으로 사용하는 것입니다. 또는 어떤 뷰는 UIKit으로, 다른 뷰는 SwiftUI로 작업하여 사용할수도 있습니다. 여기서 중요한 점은 ViewModel이 UIKit을 import하지 않는다는 것입니다. 이런 방법은 다른 플랫폼에서도 쉽게 재활용할 수 있도록 만들어줍니다.

![img](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221108044631.png)

View와 ViewModel사이의 데이터 바인딩은 클로저, delegate, observables(RxSwift)에 의해 가능합니다. 컴바인과 SwiftUI도 이를 지원하지만 iOS 13 이후에만 한정적으로 지원됩니다. View는 ViewModel과 직접적인 연관이 있고, View에서 이벤트가 발생하면 언제든 이를 전달받습니다. ViewModel은 데이터 바인딩만 이루어질 뿐, View에 대한 직접적인 참조를 가지지 않습니다.

이 예제에서는 클로저와 didSet의 조합으로 써드파티 의존성을 없애고자 하였습니다.

```swift
public final class Observable<Value> {
    
    private var closure: ((Value) -> ())?

    public var value: Value {
        didSet { closure?(value) }
    }

    public init(_ value: Value) {
        self.value = value
    }

    public func observe(_ closure: @escaping (Value) -> Void) {
        self.closure = closure
        closure(value)
    }
}
```


<br/> 

> Note: 위 코드는 Observable의 아주 간단한 버전입니다. 전체 내용을 보고싶다면 [Observable](https://github.com/kudoleh/iOS-Clean-Architecture-MVVM/blob/master/ExampleMVVM/Presentation/Utils/Observable.swift)을 참조하세요. 편의성을 위해 observer 블록은 메인 쓰레드에서 동작하도록 하였습니다. UI를 포함한 Presentation Layer에서 사용되기 때문입니다.

<br/> 

아래는 ViewController부터 데이터 바인딩을 하는 예입니다.

```swift
final class ExampleViewController: UIViewController {
    
    private var viewModel: MoviesListViewModel!
    
    private func bind(to viewModel: ViewModel) {
        self.viewModel = viewModel
        viewModel.items.observe(on: self) { [weak self] items in
            self?.tableViewController?.items = items
            // Important: You cannot use viewModel inside this closure, it will cause retain cycle memory leak (viewModel.items.value not allowed)
            // self?.tableViewController?.items = viewModel.items.value // This would be retain cycle. You can access viewModel only with self?.viewModel
        }
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        bind(to: viewModel)
        viewModel.viewDidLoad()
    }
}


protocol ViewModelInput {
    func viewDidLoad()
}

protocol ViewModelOutput {
    var items: Observable<[ItemViewModel]> { get }
}

protocol ViewModel: ViewModelInput, ViewModelOutput {}
```


<br/> 

> Note: viewModel을 observe하는 클로저 내에서 접근해서는 안됩니다. 이는 retain cycle을 발생시켜 메모리 누수를 불러옵니다. 반드시 self의 약한 참조를 통해 self?.viewModel과 같이 사용하여야 합니다.

<br/> 
아래는 TableViewCell(Reusable Cell)에 데이터 바인딩을 하는 예시입니다.

```swift
final class MoviesListItemCell: UITableViewCell {

    private var viewModel: MoviesListItemViewModel! { didSet { unbind(from: oldValue) } }
  
    func fill(with viewModel: MoviesListItemViewModel) { 
        self.viewModel = viewModel
        bind(to: viewModel)
    }
    
    private func bind(to viewModel: MoviesListItemViewModel) {
        viewModel.posterImage.observe(on: self) { [weak self] in self?.imageView.image = $0.flatMap(UIImage.init) }
    }
    
    private func unbind(from item: MoviesListItemViewModel?) {
        item?.posterImage.remove(observer: self)
    }
}
```

> Note: UITableViewCell처럼 view가 reusable하다면 unbind 작업이 필요합니다.

[MVVM Template](https://github.com/kudoleh/iOS-Clean-Architecture-MVVM/tree/master/MVVM%20Templates)

<br/> 

### MVVMs Communication

MVVM(screen)의 ViewModel은 다른 MVVM(screen)의 ViewModel과 delegate 패턴을 통해 소통합니다.

![img](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221108142817.png)

예를들어 ItemListViewModel과 ItemEditViewModel이 있을때, ItemEditViewModelDelegate 프로토콜을itemEditViewModelDidEditItem(item:) 메서드와 함께 만듭니다. 그리고 이 프로토콜을 ListItemsViewModel에 채택합니다.

```swift
// Step 1: Define delegate and add it to first ViewModel as weak property
protocol MoviesQueryListViewModelDelegate: class {
    func moviesQueriesListDidSelect(movieQuery: MovieQuery)
}
...
final class DefaultMoviesQueryListViewModel: MoviesListViewModel {
    private weak var delegate: MoviesQueryListViewModelDelegate?
    
    func didSelect(item: MoviesQueryListViewItemModel) { 
        // Note: We have to map here from View Item Model to Domain Enity
        delegate?.moviesQueriesListDidSelect(movieQuery: MovieQuery(query: item.query))
    }
}

// Step 2:  Make second ViewModel to conform to this delegate
extension MoviesListViewModel: MoviesQueryListViewModelDelegate {
    func moviesQueriesListDidSelect(movieQuery: MovieQuery) {
        update(movieQuery: movieQuery)
    }
}
```


<br/> 

> Note: Delegate의 네이밍을 Responders로 지어줄수도 있습니다. (ItemEditViewModelResponder)


<br/> 

### 클로저

또 다른 방법의 소통은 FlowCoordinator에 의해 주입되거나 할당된 클로저를 이용하는 것입니다. 이 예제 프로젝트에서는 MoviesListViewModel이 어떻게 action 클로저 showMovieQueriesSuggestions를 통해 MoviesQueriesSuggestionsView를 보여주는지가 나와있습니다. View로부터의 콜백을 위해 (_didSelect: MovieQuery)라는 파라미터도 존재합니다. 아래는 MoviesSearchFlowCoordinator 내에서 연결된 소통을 나타낸 코드입니다.

```swift
// MoviesQueryList.swift
// Step 1: Define action closure to communicate to another ViewModel, e.g. here we notify MovieList when query is selected
typealias MoviesQueryListViewModelDidSelectAction = (MovieQuery) -> Void

// Step 2: Call action closure when needed
class MoviesQueryListViewModel {
    init(didSelect: MoviesQueryListViewModelDidSelectAction? = nil) {
        self.didSelect = didSelect
    }
    func didSelect(item: MoviesQueryListItemViewModel) {
        didSelect?(MovieQuery(query: item.query))
    }
}

// MoviesQueryList.swift
// Step 3: When presenting MoviesQueryListView we need to pass this action closure as paramter (_ didSelect: MovieQuery) -> Void
struct MoviesListViewModelActions {
    let showMovieQueriesSuggestions: (@escaping (_ didSelect: MovieQuery) -> Void) -> Void
}

class MoviesListViewModel { 
    var actions: MoviesListViewModelActions?

    func showQueriesSuggestions() {
        actions?.showMovieQueriesSuggestions { self.update(movieQuery: $0) } 
        //or simpler actions?.showMovieQueriesSuggestions(update)
    }
}

// FlowCoordinator.swift
// Step 4: Inside FlowCoordinator we connect communication of two viewModels, by injecting actions closures as self function
class MoviesSearchFlowCoordinator {
    func start() {
        let actions = MoviesListViewModelActions(showMovieQueriesSuggestions: self.showMovieQueriesSuggestions)
        let vc = dependencies.makeMoviesListViewController(actions: actions)  
        present(vc)
    }

    private func showMovieQueriesSuggestions(didSelect: @escaping (MovieQuery) -> Void) {
        let vc = dependencies.makeMoviesQueriesSuggestionsListViewController(didSelect: didSelect)
        present(vc)
    }
}
```

<br/>

### Layer Separation into frameworks (Modules)

예제 앱의 각 Layer는 각각의 프레임워크로 쉽게 분리할 수 있습니다.

New Project -> Create Project -> Cocoa Touch Framework

경로로 이동하면 이러한 프레임워크들을 코코아팟을 통해 메인 앱에서 사용할 수 있습니다. 해당 예제는 [이곳](https://github.com/kudoleh/iOS-Clean-Architecture-MVVM/blob/master/MVVM%20Modular%20Layers%20Pods.zip)에서 확인할 수 있습니다.

> Note: Permission 관련 문제로 ExampleMVVM.xcworkspace를 지우고 pod install을 실행하여 새로운 파일을 만든 뒤 실행해야 합니다.

![img](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20221108154130.png)


<br/> 

### Dependency Injection Container

의존성 주입이란 하나의 객체가 다른 객체에 의존성을 가질때 이를 충족시켜주는 기술입니다. 앱의 DIContainer가 모든 injection의 중앙부 역할을 할 것입니다.

**factory 프로토콜을 위해 사용하는 의존성**

의존성 주입의 하나의 방법으로 DIContainer에 의존성을 대신 주입하는 의존성 프로토콜을 선언하는 것이 있습니다. 이를 위해선 MoviesSearchFlowCoordinatorDependencies 프로토콜을 정의하고 MoviesSceneDIContainer로 하여금 이 프로토콜을 채택하도록 만들어야 합니다. 그 후에 이를 MoviesSearchFlowCoordinator에 주입하여 MoviesListViewController를 생성하고  present해야하는 책임을 다할 수 있도록 만듭니다. 아래의 코드에 그 예시가 나와있습니다.

```swift
// Define Dependencies protocol for class or struct that needs it
protocol MoviesSearchFlowCoordinatorDependencies  {
    func makeMoviesListViewController() -> MoviesListViewController
}

class MoviesSearchFlowCoordinator {
    
    private let dependencies: MoviesSearchFlowCoordinatorDependencies

    init(dependencies: MoviesSearchFlowCoordinatorDependencies) {
        self.dependencies = dependencies
    }
...
}

// Make the DIContainer to conform to this protocol
extension MoviesSceneDIContainer: MoviesSearchFlowCoordinatorDependencies {}

// And inject MoviesSceneDIContainer `self` into class that needs it
final class MoviesSceneDIContainer {
    ...
    // MARK: - Flow Coordinators
    func makeMoviesSearchFlowCoordinator(navigationController: UINavigationController) -> MoviesSearchFlowCoordinator {
        return MoviesSearchFlowCoordinator(navigationController: navigationController,
                                           dependencies: self)
    }
}
```

**클로저의 사용**

의존성 주입의 또 다른 방법에는 클로저를 사용하는 것이 있습니다. 이를 위해선 클래스 내에 클로저를 선언하고 의존성을 이 클로저를 통해 주입합니다. 아래는 그 예시입니다.

```swift
// Define makeMoviesListViewController closure that returns MoviesListViewController
class MoviesSearchFlowCoordinator {
   
    private var makeMoviesListViewController: () -> MoviesListViewController

    init(navigationController: UINavigationController,
         makeMoviesListViewController: @escaping () -> MoviesListViewController) {
        ...
        self.makeMoviesListViewController = makeMoviesListViewController
    }
    ...
}

// And inject MoviesSceneDIContainer's `self`.makeMoviesListViewController function into class that needs it
final class MoviesSceneDIContainer {
    ...
    // MARK: - Flow Coordinators
    func makeMoviesSearchFlowCoordinator(navigationController: UINavigationController) -> MoviesSearchFlowCoordinator {
        return MoviesSearchFlowCoordinator(navigationController: navigationController,
                                           makeMoviesListViewController: self.makeMoviesListViewController)
    }
    
    // MARK: - Movies List
    func makeMoviesListViewController() -> MoviesListViewController {
        ...
    }
}
```


<br/> 

## 결론

모바일 개발에서 가장 많이 사용되는 아키텍쳐 패턴은 클린 아키텍쳐(Layer), MVVM, Redux입니다.

MVVM과 클린 아키텍쳐는 별도로 사용될 수 있지만 MVVM은 Presentation Layer에서만의 분리를 도와주는 것에 반해, 클린 아키텍쳐는 코드를 모듈화된 Layer를 통해 쉽게 테스트, 재활용, 이해가 가능하도록 합니다.

Use Case를 만드는 것은 중요한 작업이며 이를 스킵해서는 안됩니다. 설령 Use Case가 Repository를 부르는 것 이외에 아무것도 하지 않는다 하더라도 말입니다. 이 방법을 통해 아키텍쳐는 새로 온 개발자가 보았을때 self-explanatory하게 보일 것입니다.

물론 이 방법(클린 아키텍쳐)이 좋은 시작점이 될수는 있겠지만 왕도는 될 수 없습니다. (there are no silver bullets)

스스로 프로젝트의 요구에 맞게 아키텍쳐를 선택해야 합니다.

클린 아키텍쳐는 TDD와도 궁합이 좋습니다. 이 아키텍쳐는 프로젝트를 testable하게 만들어주고 Layer들이 쉽게 대체될 수 있습니다. (UI나 Data의 변경이 쉬움)

Domain-Driven Design(DDD) 또한 클린 아키텍쳐와 궁합이 좋습니다.

<br/> 

소프트웨어 개발을 할때 알아두어야 할 또 다른 5가지 원칙에 대해 소개하고 글을 마치겠습니다.

- 테스트 없이 코드를 작성하지 말 것 (TDD)
- 지속적인 리팩토링을 할 것
- 오버 엔지니어링을 하지말고 실용적이 될 것
- 써드파티 프레임워크 사용으로 인한 의존성 문제를 최소화 할 것

