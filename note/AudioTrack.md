### [AudioTrack](https://developer.android.com/reference/android/media/AudioTrack)

****

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

`AudioTrack` 相较于 `MediaPlayer` ，更贴近于底层，使用它需要手动设置音频的声道，采样等，`AudioTrack` 只支持播放 `PCM` 数据。

```java
public AudioTrack (AudioAttributes attributes, 
                AudioFormat format, 
                int bufferSizeInBytes, 
                int mode, 
                int sessionId)
```

| Parameters          |                                                              |
| ------------------- | ------------------------------------------------------------ |
| `attributes`        | `AudioAttributes`: `AudioAttributes` 实例，不能为空          |
| `format`            | `AudioFormat`: 用于描述播放的数据格式，`AudioFormat` 中包含了编码格式，声道和采样率等，不能为空 |
| `bufferSizeInBytes` | `int`: 用于读取音频数据的内部缓冲区的总大小（以byte为单位）。<br />如果 `mode` 是 `MODE_STATIC` ，其为音频最大长度；<br />如果是 `MODE_STREAM` ，其值要大于等于接收流的最小缓冲区大小，建议使用 `getMinBufferSize(int, int, int)` 方法来估算 AudioTrack的实例在流模式下的最小缓冲区大小 |
| `mode`              | `int`: 模式二选一 `MODE_STATIC` 与 `MODE_STREAM`             |
| `sessionId`         | `int`: 音频会话ID，用于将音效与播放器（AudioTrack，MediaPlayer等）关联起来，创建一个新的ID，请使用`AudioManager#generateAudioSessionId()` |

------

### AudioAttributes

封装描述音频流信息的属性集合的类，但是它没有 public 的构造方法，需要使用`AudioAttributes.Builder()` 来构造它，

```java
new AudioAttributes.Builder()             .setUsage(AudioAttributes.USAGE_MEDIA)             .setContentType(AudioAttributes.CONTENT_TYPE_MUSIC)             .build();
```

`setUsage` 设置 `AudioTrack` 的使用场景；

`setContentType` 设置输入的音频文件内容的类型；

除了以上两个常用的方法，`AudioAttributes.Builder()` 还有一些[其他方法](https://developer.android.com/reference/android/media/AudioAttributes.Builder.html)。

-----

### AudioFormat

是包含了声道信息，编码格式以及采样率的常量。同样，它也需要使用 `Builder()` 的方法构造，其中需要说明的是 `setChannelIndexMask(int channelIndexMask)` 和 `setChannelMask(int channelMask)` 都是设置声道，前者使用索引，后者使用 `AudioFormat` 的常量，例如 `AudioFormat#CHANNEL_OUT_FRONT_LEFT` ，二者设置其一就可以，代码在 `build()` 时，优先使用前者设置的数据。

### getMinBufferSize

```java
public static int getMinBufferSize (int sampleRateInHz, 
                int channelConfig, 
                int audioFormat)
```

| Parameters       |                                                              |
| ---------------- | ------------------------------------------------------------ |
| `sampleRateInHz` | `int`: 采样率，单位为 `Hz`                                   |
| `channelConfig`  | `int`: 描述音频的声道，单声道或立体声及`AudioFormat#CHANNEL_OUT_MONO` 或`AudioFormat#CHANNEL_OUT_STEREO` |
| `audioFormat`    | `int`: 音频数据表示格式，由于 `AudioTrack` 只接收 `PCM` 数据，所以格式的可选项只有:<br /> `AudioFormat#ENCODING_PCM_16BIT` 和 `AudioFormat#ENCODING_PCM_8BIT` 以及`AudioFormat#ENCODING_PCM_FLOAT` |

| 返回值 |                                                              |
| ------ | ------------------------------------------------------------ |
| `int`  | 返回在MODE_STREAM模式下创建AudioTrack对象所需的最小缓冲区大小,这是一个估计值。 |