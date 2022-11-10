---
layout: post
title: "Clean Architecture 예제 프로젝트 분석 3 - Input/Output & Action"
categories: [iOS]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## Input/Output

### Input

```swift
protocol MoviesListViewModelInput {
    func viewDidLoad()
    func didLoadNextPage()
    func didSearch(query: String)
    func didCancelSearch()
    func showQueriesSuggestions()
    func closeQueriesSuggestions()
    func didSelectItem(at index: Int)
}
```

Input은 View(유저)로부터 전달받는 이벤트들을 가지고 있음

```swift
// MARK: - INPUT. View event methods

extension DefaultMoviesListViewModel {

    func viewDidLoad() { }

    func didLoadNextPage() {
        guard hasMorePages, loading.value == .none else { return }
        load(movieQuery: .init(query: query.value),
             loading: .nextPage)
    }

    func didSearch(query: String) {
        guard !query.isEmpty else { return }
        update(movieQuery: MovieQuery(query: query))
    }

    func didCancelSearch() {
        moviesLoadTask?.cancel()
    }

    func showQueriesSuggestions() {
        actions?.showMovieQueriesSuggestions(update(movieQuery:))
    }

    func closeQueriesSuggestions() {
        actions?.closeMovieQueriesSuggestions()
    }

    func didSelectItem(at index: Int) {
        actions?.showMovieDetails(pages.movies[index])
    }
}
```

실제 구현부는 위처럼 이루어지며 view로부터 전달받은 이벤트에 대응하는것을 볼 수 있음



### Output

```swift
protocol MoviesListViewModelOutput {
    var items: Observable<[MoviesListItemViewModel]> { get } /// Also we can calculate view model items on demand:  https://github.com/kudoleh/iOS-Clean-Architecture-MVVM/pull/10/files
    var loading: Observable<MoviesListViewModelLoading?> { get }
    var query: Observable<String> { get }
    var error: Observable<String> { get }
    var isEmpty: Bool { get }
    var screenTitle: String { get }
    var emptyDataTitle: String { get }
    var errorTitle: String { get }
    var searchBarPlaceholder: String { get }
}
```

화면에 보여줄 정보들을 담고 있음

Input과 Output이 프로토콜인건 ViewModel을 testable하게 만들기 위함

Input이 모두 메서드였던 것과 다르게 Output은 모두 프로퍼티로 구성되어 있음



## Action

```swift
struct MoviesListViewModelActions {
    /// Note: if you would need to edit movie inside Details screen and update this Movies List screen with updated movie then you would need this closure:
    /// showMovieDetails: (Movie, @escaping (_ updated: Movie) -> Void) -> Void
    let showMovieDetails: (Movie) -> Void
    let showMovieQueriesSuggestions: (@escaping (_ didSelect: MovieQuery) -> Void) -> Void
    let closeMovieQueriesSuggestions: () -> Void
}
```

Action은 그 화면에서의 action들을 관리하는 장소

Coordinator에서 viewCotroller를 만들어줄때 위  struct의 인자(클로저)를 생성하여 채워넣어줌

```swift
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
        let actions = MoviesListViewModelActions(showMovieDetails: showMovieDetails,
                                                 showMovieQueriesSuggestions: showMovieQueriesSuggestions,
                                                 closeMovieQueriesSuggestions: closeMovieQueriesSuggestions)
        let vc = dependencies.makeMoviesListViewController(actions: actions)

        navigationController?.pushViewController(vc, animated: false)
        moviesListVC = vc
    }

    private func showMovieDetails(movie: Movie) {
        let vc = dependencies.makeMoviesDetailsViewController(movie: movie)
        navigationController?.pushViewController(vc, animated: true)
    }

    private func showMovieQueriesSuggestions(didSelect: @escaping (MovieQuery) -> Void) {
        guard let moviesListViewController = moviesListVC, moviesQueriesSuggestionsVC == nil,
            let container = moviesListViewController.suggestionsListContainer else { return }

        let vc = dependencies.makeMoviesQueriesSuggestionsListViewController(didSelect: didSelect)

        moviesListViewController.add(child: vc, container: container)
        moviesQueriesSuggestionsVC = vc
        container.isHidden = false
    }

    private func closeMovieQueriesSuggestions() {
        moviesQueriesSuggestionsVC?.remove()
        moviesQueriesSuggestionsVC = nil
        moviesListVC?.suggestionsListContainer.isHidden = true
    }
}
```

위의 코드에서는 viewController를 다루는 방식이 독특해서 (child view controller의 활용 등)

coordinator에서 해야하는 일인 화면전환을 다루는 것처럼 보이기도 하지만

그것과는 구분을 지어야 함

가령 화면에 tableView가 처음에는 표시되지 않다가 

search 버튼을 눌렀을때 결과를 보여주는 tableView가 나오게 하려는 경우에 

showTableView와 같은 action을 만들어서 사용하는 것

action으로 동작을 분리해야겠다는 생각이 들 때 분리해도 충분하다고 생각된다.