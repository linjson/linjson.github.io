# android_华为手机蓝牙耳机录音接收

## 开启蓝牙录音
```java
    AudioManager  mAudioManager = (AudioManager) context.getSystemService(Context.AUDIO_SERVICE);
    //以下3句是关键
    mAudioManager.setMode(AudioManager.MODE_IN_CALL);
    mAudioManager.startBluetoothSco();
    mAudioManager.setBluetoothScoOn(true);
```


## 判断头戴式蓝牙耳机或是蓝牙耳机是否已经连接
```java
    //mBluetoothAdapter在虚拟机上为null
    BluetoothAdapter mBluetoothAdapter = BluetoothAdapter.getDefaultAdapter();
    //取蓝牙耳机是否已经连接
    int a2dp = mBluetoothAdapter.getProfileConnectionState(BluetoothProfile.A2DP);
    //取头戴式蓝牙耳机是否已经连接
    int headset = mBluetoothAdapter.getProfileConnectionState(BluetoothProfile.HEADSET);
    return a2dp == BluetoothProfile.STATE_CONNECTED || headset == BluetoothProfile.STATE_CONNECTED;
```