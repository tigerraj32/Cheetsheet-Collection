## Play video

```swift
let url = Bundle.main.url(forResource: "video", withExtension:"mp4")!
let player = AVPlayer(url: url)
let vcPlayer = AVPlayerViewController()
vcPlayer.player = player
self.present(vcPlayer, animated: true, completion: nil)
```
