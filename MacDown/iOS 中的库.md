PK
    iLíSÕQ  Q    iOS ä¸­çåº.mdup viOS ä¸­çåº.md#### [iOS ä¸­çåº][3]

- A better approach is for an app to load code into its address space when itâs actually needed, either at launch time or at runtime. The type of library that provides this flexibility is called dynamic library. 

- **å¨æåº**:å¯ä»¥å¨ **è¿è¡ or å¯å¨** çæ¶åå è½½å°åå­ä¸­ï¼å è½½å°ä¸å**ç¬ç«çäº app ** çåå­å°åä¸­

- When an app is launched, the appâs codeâwhich includes the code of the static libraries it was linked withâis loaded into the appâs address space.Applications with large executables suffer from slow launch times and large memory footprints

- **éæåº**ï¼å½ç¨åºå¨å¯å¨çæ¶åï¼ä¼å° app çä»£ç (åæ¬éæåºçä»£ç ï¼ä¸èµ·å¨å è½½å° app æå¤çåå­å°åä¸ãç¸æ¯äº**éæåº** çæ¹æ¡ï¼ä½¿ç¨**å¨æåº**å°è±è´¹æ´å¤çå¯å¨æ¶é´ååå­æ¶èãè¿ä¼å¢å å¯æ§è¡æä»¶çå¤§å°ã
- Embedded frameworks are placed within an appâs sandbox and are only available to that app. System frameworks are stored at the system-level and are available to all apps.

- åµå¥ frameworks å­æ¾å¨ app çæ²çå¹¶ä¸åªå¯¹**å½å app** å¯è§ãç³»ç» frameworks å­å¨å¨ç³»ç»çº§ç®å½ï¼å¯¹**ææapp** å¯è§ã

#### [ä¸æ¶ AppStore][4] æ¶åºç°çé®é¢

- ä¸æ¶åæ æ³æç´¢å°APP
åå ï¼AppStore çæç´¢å³é®å­éè¦ç¨è±æéå·é´éï¼ç¨ä¸­æéå·åæ æ³æç´¢å°ã
è§£å³æ¹æ¡ï¼æ¹æè±æéå·ï¼éæ°åå¸çæ¬ã

- ä¸ä¼  IPA åå° AppStore æ¶ï¼æ¥éå¨æåºåå«äºä¸æ¯æçæ¡æ¶ã
åå ï¼é¡¹ç®ä¸­ä½¿ç¨ç framework åå«äº x86/i386 CPU æ¶æï¼æ¨¡æå¨éç¨ï¼ï¼iTunes Connectä¼æç» IPA åå«ææªä½¿ç¨ç [binary slices][1]ã
è§£å³æ¹æ¡ï¼å¯¹ [framework ç¦èº«][2]ãå°éæåºæåæ armv7 å arm64 æ¶æï¼ç¶åå¨åå¹¶ãç®çå¨äºåé¤å°éæåºä¸­ç x86/i386 æ¶æã


[1]:http://ikennd.ac/blog/2015/02/stripping-unwanted-architectures-from-dynamic-libraries-in-xcode/
[2]:http://honglu.me/2014/09/20/lipo%E5%91%BD%E4%BB%A4/
[3]:http://www.jianshu.com/p/48aff237e8ff
[4]:http://www.jianshu.com/p/b1b77d804254PK 
    iLíSÕQ  Q                  iOS ä¸­çåº.mdup viOS ä¸­çåº.mdPK      W       