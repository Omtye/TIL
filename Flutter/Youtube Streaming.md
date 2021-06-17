# Youtube Streaming

[Youtube Streaming.md](https://github.com/Omtye/TIL/files/6671260/Youtube.Streaming.md)

### 패키지 사용법

참고 : https://pub.dev/packages/youtube_player_flutter/install



아래 명령어를 실행합니다.

```bash
$ flutter pub add youtube_player_flutter
```





### Code

```dart
import 'package:flutter/material.dart';
import 'package:youtube_player_flutter/youtube_player_flutter.dart';

class YoutubeScreen extends StatefulWidget {
  _YoutubeScreenState createState() => _YoutubeScreenState();
}

class _YoutubeScreenState extends State<YoutubeScreen> {
  late YoutubePlayerController _controller;
  String videoId = YoutubePlayer.convertUrlToId(
          "https://www.youtube.com/watch?v=BBAyRBTfsOU")
      .toString();

  @override
  void initState() {
    _controller = YoutubePlayerController(
      initialVideoId: videoId,
      flags: YoutubePlayerFlags(
          mute: false,
          autoPlay: true,
          disableDragSeek: true,
          loop: false,
          enableCaption: false),
    );

    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
          title: Text('About'),
          centerTitle: true,
          leading: IconButton(
            icon: Icon(Icons.arrow_back),
            onPressed: () {
              Navigator.of(context).pop();
            },
          )),
      body: YoutubePlayer(
        controller: _controller,
        showVideoProgressIndicator: true,
        bottomActions: <Widget>[
          const SizedBox(width: 14.0),
          CurrentPosition(),
          const SizedBox(width: 8.0),
          ProgressBar(isExpanded: true),
          RemainingDuration(),
        ],
        aspectRatio: 4 / 3,
        progressIndicatorColor: Colors.white,
        onReady: () {
          print('Player is ready.');
        },
      ),
    );
  }
}


```

