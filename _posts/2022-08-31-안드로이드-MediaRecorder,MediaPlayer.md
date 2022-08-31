---
layout: post
title: 안드로이드 | [안드로이드] MediaRecorder,MediaPlayer
subtitle: kotlin, fastcampus
categories: 안드로이드
tags: [Android, kotlin,MediaRecorder,MediaPlayer]
---

# MediaRecorder,MediaPlayer로 오디오 녹음하고 출력하기

## MediaRecorder의 상태 다이어그램
  <br>
  <figure>
  <p align = "center"><img src="https://user-images.githubusercontent.com/41900899/187606719-5080d697-63ba-41a2-b23b-bf7f0bb4b4e4.png" width=600></p><figcaption align = "center">MediaRecorder state diagram</figcaption></figure>

  MediaRecorder는 위와 같은 상태 다이어그램을 통해 진행됩니다.
  Recording이 끝나면 다시 Initail 상태로 돌아갑니다.

## 예시 코드
  
  ~~~
  <uses-permission android:name="android.permission.RECORD_AUDIO"/>
  ~~~
  기기의 마이크에 접근해야하기 때문에 권한이 필요합니다.

  ### RECORD_AUDIO
  >RECORD_AUDIO는 사용자의 개인정보 보호에 위험을 초래할 수 있기 때문에 '위험한' 권한으로 간주됩니다. Android 6.0(API 레벨 23)부터 위험한 권한을 사용하는 앱은 런타임에 사용자에게 승인을 요청해야 합니다. 사용자가 권한을 부여하면 앱은 이를 기억하고 다시 묻지 않습니다. 

  ```kotlin
  private fun startRecording(){
        recorder = MediaRecorder().apply {
            setAudioSource(MediaRecorder.AudioSource.MIC)
            setOutputFormat(MediaRecorder.OutputFormat.THREE_GPP)
            setAudioEncoder(MediaRecorder.AudioEncoder.AMR_NB)
            setOutputFile(recordingFilePath)
            prepare()
        }
        recorder?.start()
    }
  private fun stopRecording(){
        recorder?.run {
            stop()
            release()
        }
        recorder = null
    }
  ```
  위의 startRecording()과 stopRecording()을 통해 녹음시작과 종료를 할 수 있습니다. 
  

  prepare()를 호출해야 recorder가 준비상태로 넘어가기 때문에 모든 설정을 마친 후에 prepare()를 호출해야 됩니다.


  상태 다이어그램에서 확인 할 수 있듯이 녹음이 종료되면 다시 Initial상태로 가기 때문에 설정을 매번 진행해야 합니다.
<br><br>

## MediaPlayer

  <br>
  <figure>
  <p align = "center"><img src="https://user-images.githubusercontent.com/41900899/187632778-4680eda9-8ac4-4fa8-a95b-d2b883143844.png" width=600></p><figcaption align = "center">MediaPlayer state diagram</figcaption></figure>

  MediaPlayer 또한 위와 같은 상태 다이어그램을 가집니다.

## 예시코드

  ```kotlin
    private fun startPlaying(){
        player = MediaPlayer().apply {
            setDataSource(recordingFilePath)
            prepare()
        }
        player?.setOnCompletionListener {
            stopPlaying()
        }
        player?.start()
    }

    private fun stopPlaying(){
        player?.release()
        player = null
    }
  ```
  위의 코드를 통해 간단하게 recorder에서 녹음한 오디오를 재생할 수 있습니다.
  
Player와 Recorder 모두 시스템 리소스를 잡아먹기 때문에 사용이 끝나면 release()를 해주는 것이 좋습니다.
# Reference
- [https://lovestudycom.tistory.com/209 (player에 대해서 자세하게 설명해주십니다.)](https://lovestudycom.tistory.com/209)
- [https://developer.android.com/guide/topics/media/mediarecorder?hl=ko](https://developer.android.com/guide/topics/media/mediarecorder?hl=ko)
- [https://developer.android.com/reference/android/media/MediaPlayer](https://developer.android.com/reference/android/media/MediaPlayer)
- [https://developer.android.com/guide/topics/media/mediaplayer?hl=ko](https://developer.android.com/guide/topics/media/mediaplayer?hl=ko)
- fastcampus 강의를 수강하면서 새롭게 알게 된 내용을 정리하고 있습니다.
