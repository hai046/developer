# android音频开发

使用AudioTrack

优点：灵活多变，自己可控，想播放那里就是那里，自己编解码想怎么弄就怎么弄

缺点：必须自己编解码，比MediaPlayer复杂

声明：

    private synchronized AudioTrack getAudioTracker() {

        if (mAudioTracker == null) {
            int minBufferSize = AudioTrack.getMinBufferSize(AudioLib.AUDIO_SAMPLE_RATE_IN_HZ,
                    AudioFormat.CHANNEL_OUT_MONO, AudioLib.AUDIO_FORMAT);
            mStreamType = mSensorController.getStreamType();
            mAudioTracker = new AudioTrack(mStreamType, AudioLib.AUDIO_SAMPLE_RATE_IN_HZ,
                    AudioFormat.CHANNEL_OUT_MONO, AudioLib.AUDIO_FORMAT, minBufferSize,
                    AudioTrack.MODE_STREAM);
            Log.e(TAG, "minBufferSize=" + minBufferSize);//1280
        }
        mAudioTracker.flush();
        return mAudioTracker;

    }
    
 必须注意的是获取最新的缓存区大小 minBufferSize
 这个值是根据比特率，音频格式和声道技术出来的，不是随便给出的，一般是320的整数倍
 
在android中如果你想切换播放模式 例如从听筒模式切换到外放模式的时候【修改StreamType】，必须重新初始化AudioTrack ，MediaPlayer亦然


    
    
    