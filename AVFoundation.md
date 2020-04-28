## Play video using AVPlayerViewController

> let url1 = Bundle.main.url(forResource: "video1", withExtension:"mp4")!
> let url2 = Bundle.main.url(forResource: "video2", withExtension:"mp4")!

```swift
let player = AVPlayer(url: url1)
let vcPlayer = AVPlayerViewController()
vcPlayer.player = player
self.present(vcPlayer, animated: true) {
     playerViewController.player!.play()
}
```

## Playing multiple video track simultaneously

```swift

let item1 = AVPlayerItem(url: url1)
let item2 = AVPlayerItem(url: url2)
let player = AVQueuePlayer(items: [item1, item2])
let playerViewController = AVPlayerViewController()
playerViewController.player = player
self.present(playerViewController, animated: true) {
    playerViewController.player!.play()
 }
 
 ```

