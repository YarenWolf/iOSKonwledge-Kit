### 类型检查

```
在程序运行过程中经常需要使用类型检查，使用is判断

在程序运行过程中经常需要使用类型转换，使用as转换。但是经常需要尝试解包as？，强制解包as！（如果开发者确定类型，那么可以用as！）
```

```
import UIKit

class MediaItem{
    var name:String
    init(name:String) {
        self.name = name
    }
}

class Movie:MediaItem{
    var genre:String
    init(name:String,genre:String) {
        self.genre = genre
        super.init(name: name)
    }
}

class Music:MediaItem{
    var artist:String
    init(name:String, artist:String) {
        self.artist = artist
        super.init(name: name)
    }
}

class Game:MediaItem{
    var developer:String
    init(name:String,developer:String) {
        self.developer  = developer
        super.init(name: name)
    }
}

//多态性
//多态一般需要向下转换
let library:[MediaItem] = [
    Movie(name: "Zootopia", genre: "Animation"),
    Music(name: "Hello", artist: "Adele"),
    Game(name: "LIMBO", developer: "Playdead"),
    Music(name: "See you again", artist: "Wiz Hkaifa"),
    Game(name: "The Bridge", developer: "The Quantum Astrophysicists Guild")
]

//类型转换 : as
//类型转换为了避免失败，用as？尝试解包，如果开发者确定一定类型一定正确那么可以使用as！强制解包
if let movie = library[0] as? Movie{
    movie.name
    movie.genre
}

var movieCount:Int = 0
var musicCount:Int = 0
var gameCOunt:Int = 0
for media in library{
    //运行过程中类型检查 : is
    if media is Movie {
        movieCount += 1
    }else if media is Music{
        musicCount += 1
    }else{
        gameCOunt += 1
    }
}

movieCount
musicCount
gameCOunt


for mediaItem in library {
    if  let movie = mediaItem as? Movie {
        print("Movie:",movie.name,"Genre:",movie.genre)
    }else if let music = mediaItem as? Music{
        print("Music:",music.name,"Artist:",music.artist)
    }else if let game = mediaItem as? Game{
        print("Game:",game.name,"Developer:",game.developer)
    }
}
```



