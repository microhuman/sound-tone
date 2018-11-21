# sound-tone
声音变调的播放实现，再安卓系统音频LoudnessEnhancer扩展出来的功能. 核心处理代码来自于 http://www.surina.net/soundtouch/


##android source code:

#system\media\audio_effects\include\audio_effects\effect_loudnessenhancer.h
Add:
LOUDNESS_ENHANCER_PARAM_TARGET_TONE = 1

#frameworks\av\media\libeffects\loudness\EffectLoudnessEnhancer.cpp
Add:
LE_init:
pContext->pSoundTouch->setSampleRate(sampleRate);
pContext->pSoundTouch->setChannels(channels);

LELib_Create:
pContext->pSoundTouch = NULL;

LE_process:
pContext->pSoundTouch->putSamples(inBuffer->s16, inBuffer->frameCount);
pContext->pSoundTouch->receiveSamples(pContext->tempBuff, outBuffer->frameCount);

LE_command:
case EFFECT_CMD_GET_PARAM:
  LOUDNESS_ENHANCER_PARAM_TARGET_TONE
EFFECT_CMD_SET_PARAM:
  LOUDNESS_ENHANCER_PARAM_TARGET_TONE
  
frameworks\base\media\java\android\media\audiofx\LoudnessEnhancer.java
Add:
public static final int PARAM_TARGET_TONE = 1;

public void setTone(int vol)
      throws IllegalStateException, IllegalArgumentException, UnsupportedOperationException {
        checkStatus(setParameter(PARAM_TARGET_TONE, vol));
}

public int getTone()
    throws IllegalStateException, IllegalArgumentException, UnsupportedOperationException {
    int[] value = new int[1];
    checkStatus(getParameter(PARAM_TARGET_TONE, value));
    return value[0];
}
    
frameworks\base\media\jni\audioeffect\android_media_AudioEffect.cpp


complie:
cd  android_sourcecode/
buildenv
lunch
make update-api  #first
make 

wish you luck
