�ֲ�ʽ�����һ��ʵ�ַ�ʽ��������״̬��ת

[TOC]

���ڷֲ�ʽ���񣬲ο��������ᵽ��һЩ�취������������Ϣ����ʵ�ֲַ�ʽ���񣬲�������TCC�����Ŭ���ʹ�ȵȡ���������Լ���һЩ����ʵ�֡����Գ�֮Ϊ״̬��ת��ʵ�֡�

## һЩҪ��
- �������ֳɶ��С����ÿ��С�����ǵ����ϵ�����
- Ҫ֧���ݵȣ���ÿ��С������ִ��ʱ���Ҫ��ͬ��
 - ��δﵽ�ݵȣ�һ���Ǽ�һ��״̬����һ������״̬�ֶε��У���д��ҵ������ʱͬʱ�޸�״̬Ϊ�������������ѳ�����֮��ġ��´�����ʱ���ȼ��״̬��ֵ�����������Ϊ�������������������ˣ���������ڣ�˵���ǡ�δ��������ʱ��Ҫд��ҵ�����ݡ�
- ��Ӧһ��������Ķ��С������α�֤ȫ���ɹ�����ȫ��ʧ�ܡ�
 - �ȷ�����α�֤ȫ���ɹ������Ƕ������Σ�ֱ��ȫ���ɹ����������ݵȵı�֤��������ĳЩС��������˼��Σ��ǲ�Ӱ�����ݵ���ȷ�Եġ�
 - ͬ����Է���ȫ��ʧ�ܼ���Ҫ�ع��������Ҳ�����ݵȱ�֤��ǰ���¶������Σ�ֱ��ȫ���ع���ɡ�
 - ��α�֤�������Σ��취Ӧ�úܶ࣬һ�ַ������ڵ�һ��С����ִ��ǰ�ȷ�һ����ʱ��Ϣ�������Ϣ�а���Ԥ�����ɵĶ�Ӧ�������������̵�id���������㵱ǰ������������ĳ������ʧ�ܣ��Ժ�Ҳ���ٴμ��ִ�еĻ��ᡣ  
 - ������һЩ���ɡ�ʹ��Ψһ������֤������Ψһ���Լ�������������ʹ��CAS(compare and set)���������޸Ĳ�������ר��һ��rowver�ֶΡ�

***

## ����һ���򵥵����ӣ�ת��ҵ��  


- ��صı�����  
 - Account(userId,amount,rowver)  
 - AccountTransferState(acsId,idForPart,state,fromUserId,toUserId,amount,rowver)  
     - ע��AccountChangeState��2�ݼ�¼��ֻ��idForPart��ͬ��ע��������ͬ����acsId����ͬ�����ֱ�ΪfromUserId��toUserId��������Account��һһ��Ӧ�ģ����ҷ�����ʽ��ͬ����������������  
  
### �ȷ��������е����������  
- 0,Ԥ������acsId����������Ϣ�����У���Ϣ���ݰ���AccountTransferState�ļ��������ֶ� �Լ� msgType=checkTransfer(�ڱ������»�����msgType=newTransfer���൱���첽����ת�˲���)�ȡ�  
 - ������Ϣ�������Զ������������˹���Ԥ�����磬�û�������ָfromUser�����Բ�ѯһ��AccountTransferState���б���ʱ��������������������¼δ��ɣ����Ե㰴ť����������  
 - ����2���������Ԥ������acsId�󣬶��а취�õ����acsId�Լ�����������ݣ���fromUserId�ȵȣ���  
- 1,�ڵ���������,�ȼ��fromAccount��amount��  
 - �����������ֱ�ӷ��أ�������������fail������  
 - �����������fromAccount �� �½�AccountTransferState(acsIdΪԤ������,idForPart=fromUserId,state=didOut)��  
- 2,�ڵ��������� ����toAccount �� �½�AccountChangeState(idForPart=toUserId,state=finish)  
- 3,�ڵ��������� �޸�idForPart=fromUserId��AccountTransferState��ʹstate=finish��  

### �ٷ��������е��쳣�����  
&emsp;&emsp;��1��2��3��ÿһ�������ܳ��쳣������0�����쳣������ܼ򵥣��ο������1B���輴�ɣ�    
&emsp;&emsp;���ڵ�0�����ڣ����а취�õ������id�ͱ�Ҫ���ݡ��Ӷ����Է�����������Ĺ��̣����¡�  

- 1B,�ڵ���������,ʹ��acsId��idForPart=fromUserId��ѯAccountTransferState��¼��  
 - ��������ڣ�˵����1�����������ڸ���ԭ��ʧ�ܣ��Ӷ�����������ʧ�ܣ����ù��ˡ�������Ҳ���Կ����ٴ�ת�ˣ������������ݲ��������Ա�������һЩ����Ҫ�ĸ����ԣ�    
 - ������ڣ���AccountTransferState��¼���ݣ�  
     - ���state=finish����˵�����ڵ�3��������ɺ�����쳣����ʱ���������Ѿ��ɹ���ɣ�������������ʲô��    
     - ���state=didOut��˵����1���Ѿ���ɣ�����һ����������2B.    
     - ���state=��������ҵ���߼�������Ӧ�ó��֣���������Ա��bug��    
- 2B, �ڵ���������,ʹ��acsId��idForPart=toUserId��ѯAccountTransferState��¼��    
 - ��������ڣ�˵����2����δ��ʼ��δ�ܳɹ���ʼ��ֻ�����ǰ���2��3���е�����    
 - ������ڣ���AccountTransferState��¼���ݣ�    
     - ���state=finish��˵����2���Ѿ��ɹ���ɣ�ֻ�����ǰ��ĵ�3���е�����  
     - ���state=��������ҵ���߼�������Ӧ�ó��֣���������Ա��bug��  
- 3B,ͬǰ��ĵ�3��  
 - �ⲽ�Ƚϼ򵥡���������ķ���������ܹ����ⲽ��ֻ�е�ǰ��2�����ɹ�ʱ�ŵ������������ж�ʲô������ֻ���ִ�е�3�������ݼ��ɡ�  




***
## ����һ���Ƚϸ��ӵ����ӣ�ʹ�û��ֺ�������ֺ�����ڲ�ͬ�Ŀ��С�
�����ڲ�ͬ�Ŀ��У������԰�һ��С��������һ��Զ�̵��á�  

	��صı�����  
	����AccountTransferRequest��4��Accountһ����5Ƭ��  
	AccountTransferRequest(atrId,fromUserId,toUserId,state,crtTime,detailJson,shuoming,rowver)//detailJson ���� ������¼��ÿ����¼�� AccountType, amount  
	AccountYuE(userId,amount,rowver)  
	AccountJiFen(userId,amount,rowver)  
	AccountYuEChangeState  (userIdForPart,atrId,state,rowver, amount)  
	AccountJiFenChangeState(userIdForPart,atrId,state,rowver, amount)  
		ע��AccountXyzChangeState��2�ݼ�¼���ֱ���2��AccountXyz��Ӧ���ҷ�����ʽ��ͬ��������֤��������
	
    ����ִ����������ʱ(��һ��Ҳ�ƴ����׶�)
	1,����ת������ AccountTransferRequest(state=Init)
		������ǰԤ������atrId,(�������ȱ��浽������������ĸ������ϣ���������׷��״̬)��
		��һ��ִ�����Ҳ��÷ֲ�ʽ������ΪatrId�������ɵģ�û�б�ĵط����á�����������������id��
		������ʱ��Ϣ������(1minute,msgType=checkTransfer)����Ϣ���ݰ���AccountTransferRequest�ļ��������ֶΣ��ص����atrId,fromUserId,msgType=checkTransfer(���첽����ת�˲�����������msgType=newTransfer)��
		��һ���棬�û���������fromUser��������by fromUserId��ѯһ��AccountTransferRequest���б���ʱ��������������������¼δ��ɣ����Ե㰴ť���������������ߴ�ĳ���������õ�atrId������һ��ȡ�����ݣ�
		����2��������������õ�atrId��fromUserId��
		���⣬toUser��ʱ��Ϊû�в�ѯAccountTransferRequest���б�ı�Ҫ��
			һ���棬ûת�˳ɹ���toUser����֪���������״̬���м����(�Ͼ����ڵĵ�����վҲû���ṩ���ֲ�ѯ)��
			��һ���棬�����Բ�AccountChangeState��Ϊ�б�����join���⣬�������༸���ֶλ�һ��json�ַ����ֶΣ����ߵ����������ת���칹���ݿ��������ѯ���⡣
	2,���� AccountJiFen ��ת�������߼�
		�ȼ��fromAccountJiFen��amount��������
			�� ���� ����fromAccountJiFen �� �½�AccountJiFenChangeState(fromUserId,atrId,state=didOut,atrDetailJson~)
			�������� ��дfromAccountJiFen��AccountJiFenChangeState�����ݡ�
				��ʱ��Ҫ�ع����ȷ�����ʱ��Ϣ�����У���Ϣ���ݰ���AccountTransferRequest�ļ��������ֶΣ��ص����atrId,msgType=toRollback��
					���������ڷ�����Ϣǰ���쳣ͣ�����´�ͨ����Ϣ�����鴦�����̣������Բ���2�������߹��벻��2����֧�������ᵼ�����ݲ�һ�µȴ���
				Ȼ���޸�AccountTransferRequest��state=toRollback�����޸�shuoming�ֶ�Ϊʧ��ԭ�������ǻ��ֲ��㡣Ȼ�󷵻�ʧ��ԭ����ʾ�û����߷���Ϣ֪ͨ�û���
				Ȼ��ִ��B���Ļع��߼���������ִ��������߼������ǵ�ͨ���ԣ�B���Ļع��߼������ദ���ݵĴ���
				ע�����������з�֧·������Ҫ���Ⲣ������Ϊ���ų���ĳЩ����£���ͬ��������Ϣ��2���̲߳�����������2���߳��ߵķ�֧��һ����������ִ�еķ���amount������Ҫ�ع�������ִ�еķ���amount���ˣ�����֮ǰamount����һ���߳������ˣ�������ִ��������֧������Ҫһ�ѷֲ�ʽ�������ڴ������������Ⲣ����
	3,���� AccountYuE ��ת�������߼�
		�ȼ��fromAccountYuE��amount��������
			�� ���� ����fromAccountYuE �� �½�AccountYuEChangeState(fromUserId,atrId,state=didOut,atrDetailJson~)
			�������� ��дfromAccountYuE��AccountYuEChangeState�����ݡ�
				�����޸�AccountTransferRequest��state=toRollback�����޸�shuoming�ֶ�Ϊʧ��ԭ�����������㡣Ȼ�󷵻�ʧ��ԭ����ʾ�û����߷���Ϣ֪ͨ�û���
				Ȼ��ִ��B���Ļع��߼���������ִ��������߼������ǵ�ͨ���ԣ�B���Ļع��߼������ദ���ݵĴ���
				ע�����������з�֧·������Ҫ���Ⲣ����
	4,���� AccountJiFen ��ת�벿���߼����������� ����toAccountJiFen �� �½�AccountJiFenChangeState(toUserId,atrId,state=didIn,atrDetailJsonStr~)
		�˴��������ҵ���߼��������Ҫ�ع������������Ӧϵͳͣ��ά������ʱ�������Ӧ�ûع�������ֻ��һֱ����ֱ���ɹ����ɣ���Ȼ���Ի�����Ҫϸ�����ǡ�
	5,���� AccountYuE ��ת�벿���߼����������� ����toAccountYuE �� �½�AccountYuEChangeState(toUserId,atrId,state=didIn,atrDetailJsonStr~)
		�˴��������ҵ���߼��������Ҫ�ع������������Ӧϵͳͣ��ά������ʱ�������Ӧ�ûع�������ֻ��һֱ����ֱ���ɹ����ɣ���Ȼ���Ի�����Ҫϸ�����ǡ�
	6,�޸�ת������AccountTransferRequest�� state=finish
	7,���ޱ�Ҫ�޸�4��AccountChangeState��state=finish���������ݲ�����
	----
	B �ع��߼�
	B0 �ȼ�һ���ֲ�ʽ��������Redisson����atrIdΪkey���������Է�ֹ����Ĳ���ִ�С�
	B1 �ȷ�����ʱ��Ϣ�����У���Ϣ���ݰ���AccountTransferRequest�ļ��������ֶΣ��ص����atrId,msgType=toRollback���Ա�֤���沽����쳣ʱ�����ܼ������ع�����ֱ����ɡ�
		���AccountTransferRequest��state!=toRollback������state=Init�����޸�AccountTransferRequest��state=toRollback��
		���state=finish OR fail �������ǲ����κ����飬����Ϊ����������·���Ǻ���ֵģ���Ҫ�ÿ��������Ƿ����bug��
	B2 from���� AccountJiFen�Ļع�
		���� atrId �� fromUserId ��ѯ AccountJiFenChangeState ��¼
			���û�鵽��¼��˵��û��ת�����������ûع����������κβ�����
			����鵽1����¼���������ܶ�����¼�������������bug��
				���state=didOut������Ҫ�ع�������fromAccountJiFen �� �޸�AccountJiFenChangeState��state=didRollback ��
				���state=didRollback��˵���Ѿ��ع��ˣ����������κβ����ˡ�
	B3 from���� AccountYuE�Ļع�
		���� atrId �� fromUserId ��ѯ AccountYuEChangeState ��¼
			���û�鵽��¼��˵��û��ת�����������ûع����������κβ�����
			����鵽1����¼���������ܶ�����¼�������������bug��
				���state=didOut������Ҫ�ع�������fromAccountYuE �� �޸�AccountYuEChangeState��state=didRollback ��
				���state=didRollback��˵���Ѿ��ع��ˣ����������κβ����ˡ�
	B4 to���� AccountJiFen�Ļع�
		���� atrId �� toUserId ��ѯ AccountJiFenChangeState ��¼
			���û�鵽��¼��˵��û��ת�붯�������ûع����������κβ�����
			����鵽1����¼���������ܶ�����¼�������������bug��
				���state=didIn������Ҫ�ع�������toAccountJiFen �� �޸�AccountJiFenChangeState��state=didRollback ��
				���state=didRollback��˵���Ѿ��ع��ˣ����������κβ����ˡ�
	B5 to���� AccountYuE�Ļع�
		���� atrId �� toUserId ��ѯ AccountYuEChangeState ��¼
			���û�鵽��¼��˵��û��ת�붯�������ûع����������κβ�����
			����鵽1����¼���������ܶ�����¼�������������bug��
				���state=didIn������Ҫ�ع�������toAccountYuE �� �޸�AccountYuEChangeState��state=didRollback ��
				���state=didRollback��˵���Ѿ��ع��ˣ����������κβ����ˡ�
	B6 ����޸�AccountTransferRequest��state=fail
	B7 �ͷŷֲ�ʽ��
	----
	������1--6��ÿһ��������ʧ�ܻ��쳣���о�����ʧ�������
		���ڿ����õ������id�������ȷ�2�������һ������ȫ��������ʱ��Ӧ����Ĳ��裬�򵥡�
		һ����ĳ������ʧ�ܺ�ͨ��ĳ�ַ�ʽ�������ת�����̣����������ۡ�
	
	0C,�ȼ�һ���ֲ�ʽ��������Redisson����atrIdΪkey���������Է�ֹ����Ĳ���ִ�С�����Ϊ��Ҫ���Ⲣ����ǰ���еط���������
	1C,ʹ��atrId��ѯAccountTransferRequest��¼��
		��������ڣ�˵����1����û�ɹ���������ȫ�·�ʽ���������1--6�����ݲ������Ѿ����ش�����Ϣ���û������û����������������κζ����������
		������ڣ���AccountTransferRequest��¼���ݣ�
			���state=finish����ʱ���������Ѿ��ɹ���ɣ�������ʲô��ֻ�����ѵ�������Ϣ���ɡ�
			���state=fail�����������Ѿ�ʧ�ܣ�Ҳ���ù��ˡ�
			���state=toRollback��ת�� B �ع��߼� �Ĵ�������
			���state=Init��˵�����ٵ�1���Ѿ���ɣ��ɵ�2������鴦����2C.
			���state=��������ҵ���߼�������Ӧ�ó��֣���������Ա����������bug��
	2C, ʹ��atrId��idForPart=fromUserId��ѯAccountJiFenChangeState��¼��
		��������ڣ�˵����2����δ��ʼ��δ�ܳɹ���ʼ������2--6����
			�����п���֮ǰ�ǵ�2�����ʧ�ܣ�����δ���޸�AccountTransferRequest��¼״̬��ע����ܳ�����ǰ���ʧ�ܣ����ڼ����Գɹ����������Ȼ������û�����⣬���ǲ�����������������Ҫ��ֹ������
		������ڣ���AccountJiFenChangeState��¼���ݣ�
			���state=didOut��˵����2���Ѿ��ɹ���ɣ��ɵ�3������鴦����3C.
			���state=didRollback,˵��Ӧ�ûع��ˣ�ת�� B �ع��߼� �Ĵ�������
			���state=��������ҵ���߼�������Ӧ�ó��֣���������Ա����������bug��
	3C, ʹ��atrId��idForPart=fromUserId��ѯAccountYuEChangeState��¼��
		��������ڣ�˵����3����δ��ʼ��δ�ܳɹ���ʼ������3--6����
			�����п���֮ǰ�ǵ�3�����ʧ�ܣ�����δ���޸�AccountTransferRequest��¼״̬��ע����ܳ�����ǰ���ʧ�ܣ����ڼ����Գɹ����������Ȼ������û�����⣬���ǲ�����������������Ҫ��ֹ������
		������ڣ���AccountYuEChangeState��¼���ݣ�
			���state=didOut��˵����3���Ѿ��ɹ���ɣ��ɵ�4������鴦����4C.
			���state=didRollback,˵��Ӧ�ûع��ˣ�ת�� B �ع��߼� �Ĵ�������
			���state=��������ҵ���߼�������Ӧ�ó��֣���������Ա����������bug��
	4C, ʹ��atrId��idForPart=toUserId��ѯAccountJiFenChangeState��¼��
		��������ڣ�˵����4����δ��ʼ��δ�ܳɹ���ʼ������4--6��
		������ڣ���AccountJiFenChangeState��¼���ݣ�
			���state=didOut��˵����4���Ѿ��ɹ���ɣ��ɵ�5������鴦����5C.
			���state=didRollback,˵��Ӧ�ûع��ˣ�ת�� B �ع��߼� �Ĵ�������
			���state=��������ҵ���߼�������Ӧ�ó��֣���������Ա����������bug��
	5C, ʹ��atrId��idForPart=toUserId��ѯAccountYuEChangeState��¼��
		��������ڣ�˵����5����δ��ʼ��δ�ܳɹ���ʼ������5--6��
		������ڣ���AccountYuEChangeState��¼���ݣ�
			���state=didOut��˵����5���Ѿ��ɹ���ɣ��ɵ�6������鴦����6C.
			���state=didRollback,˵��Ӧ�ûع��ˣ�ת�� B �ع��߼� �Ĵ�������
			���state=��������ҵ���߼�������Ӧ�ó��֣���������Ա����������bug��
	6C,ͬ��6��
	7C,�ͷŷֲ�ʽ��

***

## С��
����״̬��ת�ķֲ�ʽ�����ʵ�ֵĺô�������Ϊ���ĺô�����ֱ�ۡ�
���е����ݴ��������Ժ���һ����ĺ������棬���Կ�������������̵�ȫò��
����Ҳ�������˶�ǰ���2�����ӵľ������ʵ�֣��� https://github.com/zlywq/tmycat1 ��


�������������Ͱ�
![�������������Ͱ�](http://jianpuapk.oss-cn-hangzhou.aliyuncs.com/my/weixinPayQRcodeZlywq.png)








***

