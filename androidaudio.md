# android_audio

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