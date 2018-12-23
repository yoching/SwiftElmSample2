# Elm Button Sample implemented with Swift

This is a swift implementation of [Buttons sample](https://guide.elm-lang.org/architecture/buttons.html) in Elm Official Guide.  
For simplicity, [commands and subscriptions](https://guide.elm-lang.org/effects/) are not included.  
Also, incremental updates of views are not implemented.  
(If you want to know them, please refer to [referenced materials](#References).)  

I hope this will be a help to start learning The Elm Architecture with swift!

![](./ButtonsSample.gif)

## AppState
```swift
struct AppState {

    // MODEL
    var value: Int

    // UPDATE
    enum Message {
        case increment
        case decrement
        case save
        case load
        case loaded(Int)
    }

    mutating func update(_ message: Message) -> [Command<Message>] {
        switch message {
        case .increment:
            value = value + 1
            return []
        case .decrement:
            value = value - 1
            return []
        case .save:
            return [.save(value: value)]
        case .load:
            return [.load(available: { .loaded($0) })]
        case .loaded(let value):
            self.value = value
            return []
        }
    }

    // SUBSCRIPTIONS
    var subscriptions: [Subscription<Message>] {
        return [
            .notification(
                name: UIApplication.didBecomeActiveNotification,
                { notification -> Message in
                    return .load
            })
        ]
    }

    // VIEW
    var viewController: ViewController<Message> {
        return ._viewController(
            .stackView(
                views: [
                    .button(text: "-", onTap: .decrement),
                    .label(text: "\(value)"),
                    .button(text: "+", onTap: .increment),
                    .button(text: "save", onTap: .save),
                    .button(text: "load", onTap: .load)
                ],
                axis: .vertical,
                distriburtion: .fillEqually
            )
        )
    }
}
```


## References
- App Architecture from objc.io
  - [book & video](https://www.objc.io/books/app-architecture/)
  - [sample codes](https://github.com/objcio/app-architecture)
    - especially, Recordings-TEA & One-App-Eight-Architectures
