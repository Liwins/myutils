# �������
## ����
asio  
nlohmann::json  
spdlog  
sqlite_modern_cpp  
ccronexpr  
concurrentqueue  
httplib  
ThreadPool 
## ʹ�÷���
### ѩ��ƬUUID���ɹ���
TODO:
###  String������  
```
    using namespace rs;
    auto apppath = StringUtils::getAppPathRS();
    std::cout << apppath << std::endl;
    std::string a = "sgx@uc2012@12@Busf@f@@";
    auto aVec = StringUtils::Split(a, "@");
    auto bVec = StringUtils::Split(a, "@", true);
    std::cout << aVec.size() << std::endl;
    std::cout << bVec.size() << std::endl;
    std::cout << StringUtils::Join(aVec, "*") << std::endl;
    std::cout << StringUtils::PathSeparatorRS() << std::endl;
    auto timeA = StringUtils::convFromStr("2020-05-18T10:50:00");
    std::cout << timeA << std::endl;
    std::cout << StringUtils::ToUpper(a) << std::endl;
    std::cout << StringUtils::ToLower(a) << std::endl;
```
### clock���ܲ���  
ʹ�÷���ջ��ʹ��
```
    using namespace rs::clock;
    auto timerClockPtr = TimerClockFactory::getInstance();
    std::this_thread::sleep_for(std::chrono::seconds(3));
    std::cout << timerClockPtr->elapsed_second() << std::endl;
    timerClockPtr->reset();
    std::this_thread::sleep_for(std::chrono::milliseconds(3));
    auto reult = timerClockPtr->elapsed();
    std::cout << reult << std::endl;
```
### ByteBuffer
�ֽ�����  
TODO:  
### cron֧��
```
    using namespace rs::quart;
    std::string cron = "0 0 10 * * *";
    std::chrono::system_clock::time_point ti;
    if (getNextTimePoint(cron, ti))
    {
    	auto tim = std::chrono::system_clock::to_time_t(ti);
    	auto reString = rs::StringUtils::TimeToString(tim);
    	std::cout << reString << std::endl;
    }
```
### web֧��
����Լ���������õķ�ʽ,Ĭ�Ͽ���80�˿�,��������а�,�򲻻�ʹ�øÿ��,Ҳ�����ж����߳�����
```
void getDemo1(const httplib::Request& req, httplib::Response& resp)
{
	nlohmann::json f{ "msg",{"data","helloword"} };
	std::string result = f.dump();
	resp.set_content(result, "application/json");
}
void postDemo(const httplib::Request& req, httplib::Response& resp)
{
	auto reqBody = req.body;
	resp.set_content(reqBody, "application/json");
}
void testWeb()
{

	rs::web::Bind<true>("/api/getDemo", getDemo1);
	rs::web::Bind<false>("/api/postDemo", postDemo);
}
```
### log֧��
ʹ��ǰ��Ҫע���޸�rs::log�е�getLogger��ʼ���е�callOnce,ָ�����õ�������log��ʽ.����������κ��޸�,Ĭ��ʹ�ÿ���̨��־.
Ŀǰ��3��,1 only cmd 2 daylog+cmd 3 rotating+cmd
����2,3 ����configĿ¼�µ�log.json����  
daylog����
```
{
"logName":"system.log",
"hour":1,
"min":1,
"fileLevel":"debug",
"cmdLevel":"info"
}
```
rotating���� 
```
{
"logName":"system.log",
"fileNum":3,
"maxSize":100,
"fileLevel":"debug",
"cmdLevel":"info"
}
```
```
using namespace rs::log;
static auto mainLog = rs::log::getLogger("main");
mainLog->info("cmd log :{}", 23);
```
### dumpbin
������winƽ̨
```
int main(int argc, char* argv[])
{
	SetUnhandledExceptionFilter(rs::dumpbin::ExceptionFilter);
}
```
### zabbix֧��
��ҪĬ��������
��zabbixĿ¼��,����zabbix.json
```
{
  "ZabbixHost": "192.168.1.2",
  "ZabbixPort": 10051,
  "MonitoringHost": "192.168.1.159",
  "MonitoringKey": "sgxMarket"
}
```
ʹ�÷���:
zabbix����ռ��һ���߳�,��������������˳�,��ô�ᷢ��������ȥ�����,���������صķ�ʽ,����Ҫ���Ĳ�ʹ�ø�����ᴴ���ö���
```
	rs::zabbix::send("asdf");
	std::this_thread::sleep_for(std::chrono::seconds(3));
```