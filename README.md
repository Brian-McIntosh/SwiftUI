# SwiftUI Code Snippets

### ContentView:
```swift
struct ContentView: View {
    
    // this i've seen
    @StateObject private var todoListVM = ToDoListViewModel()
    
    var body: some View {
        NavigationView {
            
            VStack {
                // this i've seen
                List(todoListVM.todoItems) { todoItem in
                    Text(todoItem.title)
                }
            }.onAppear {
                // this i've seen
                todoListVM.populateToDos()
            }
        }
    }
}
```

### ViewModel:
```swift
class ToDoListViewModel: ObservableObject {
    
    @Published var todoItems = [ToDoItemViewModel]()
    
    func populateToDos() {
        
        WebService().getAllToDos(url: Constants.Urls.allToDosURL) { result in
            switch result {
            case .success(let todos):
                DispatchQueue.main.async {
                    self.todoItems = todos.map(ToDoItemViewModel.init)
                }
            case .failure(let error):
                print(error.localizedDescription)
            }
        }
    }
    
}
```
