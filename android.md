# Android ������¼

## adbʹ��

### �����豸

> adb tcpip 5555  //����tcpip���Զ˿�
> adb pair 192.168.x.x:port             //��Ҫ��ͬһ����,�������ֻ��ҵ����ߵ���ѡ��ҵ��˿�,���������Ȩ
> adb connect  192.168.x.x:debug-port  //�Լ����豸��ַ,������ͬһ����
> adb devices        //�鿴�豸�б�
> adb unroot/root             // �Է�/rootģʽ���� , -sָ���豸���
> abd remount     //���¹��ط�����д
> abd shell            //��¼���豸��shell
> adb -s -r �豸��� uninstall/install apk    //ж��/��װ���,-s�ǰ�װ��sd��,-r�Ǹ��ǰ�װ.���������װһֱʧ��,���Լ���-tѡ��,����test
> adb reboot  //����
> adb start-server  //����adb server
> abd kill-server    //ֹͣadb server
> adb shell pm clear package-name    //�������ݺͻ���
> adb shell dumpsys activity activities | grep mFocusedActivity  //�鿴ǰ̨Activity
> adb shell dumpsys activity services  packagename   //�鿴�������е�services ,�����ɲ�д
> adb shell dumpsys package packagename      //�鿴Ӧ�õ���ϸ��Ϣ
> adb shell input  opetion  //ģ�����
> adb am/pm     //ҳ��Ͱ���������
> adb pull/push  //����/�ϴ�����
> adb disconnect  //�Ͽ�����
> adb shell am start -n packagename  /packagename.activity  //����һ��Ӧ�õ�activity
> adb shell am start -n packagename/.activity      //�������ʽ�ļ�д
