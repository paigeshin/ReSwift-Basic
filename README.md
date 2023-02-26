# ReSwift-Basic

```swift
import ReSwift
import ReSwiftThunk
import SwiftUI

struct AppState {
    var latitude: Double?
    var longitude: Double?
    var weatherData: Result<String, Error>?
}

let thunkMiddleware: Middleware<AppState> = createThunkMiddleware()

let appStore = Store<AppState>(reducer: appReducer,
                               state: AppState(),
                               middleware: [thunkMiddleware])

enum AppAction: Action {
case fetchWeatherFor(weather: Result<String, Error>)
case getUserLocation(latitude: Double, longitude: Double)
}


func appReducer(action: Action, state: AppState?) -> AppState {
    var state = state ?? AppState()
    guard let action = action as? AppAction else {
        return state
    }
    
    switch action {
    case .fetchWeatherFor(weather: let weather):
        state.weatherData = weather
    case .getUserLocation(latitude: let lat, longitude: let long):
        state.latitude = lat
        state.longitude = long
    }
    return state
}

let fetchWeatherThunk = Thunk<AppState> { dispatch, getState in
    if let state = getState(),
       let lat = state.latitude,
       let lo = state.longitude {
        
        DispatchQueue.main.asyncAfter(deadline: .now() + 3) {
//            dispatch(AppAction.fetchWeatherFor(weather: ))
        }
        
    }
}


class ViewController: UIViewController, StoreSubscriber {

    override func viewDidLoad() {
        appStore.subscribe(self)
    }
   
    override func viewDidDisappear(_ animated: Bool) {
        appStore.unsubscribe(self)
    }
    
    func newState(state: AppState) {
        // updated state...
        appStore.dispatch(fetchWeatherThunk)
    }
    
}

```
