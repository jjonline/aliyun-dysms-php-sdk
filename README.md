# aliyun-dysms-php-sdk
阿里云-云通信PHP版sdk精简，非原先阿里大于SDK，原阿里大鱼被合并进了“阿里云-云通信”。

“阿里云-云通信”的SDK已变化，原先阿里大鱼的SDK已无法使用，此git库为新版“阿里云-云通信”提供的SDK打成composer包

为了便于composer管理，特制作该git库，版权归阿里所有，本人仅处理、精简、适配自己业务的部分代码；详情请查看各commit


> v2.0版做了精简，去除了单元测试和相关文件，v1.0版保留原SDK核心


## composer安装

`composer require jjonline/aliyun-dysms-php-sdk`


*若需要原SDK核心，请使用v1.0版*


composer安装后用法就跟官方文档没什么区别了，做个示例：

    use Aliyun\Core\Config as AliyunConfig;
    use Aliyun\Core\Profile\DefaultProfile;
    use Aliyun\Core\DefaultAcsClient;
    use Aliyun\Api\Sms\Request\V20170525\SendSmsRequest;
    use Aliyun\Api\Sms\Request\V20170525\QuerySendDetailsRequest;
    
    // 阿里云Access Key ID和Access Key Secret 从 https://ak-console.aliyun.com 获取
    $appKey = '你的Access Key ID';
    $appSecret = '你的Access Key Secret';
    
    // 短信签名 详见：https://dysms.console.aliyun.com/dysms.htm?spm=5176.2020520001.1001.3.psXEEJ#/sign
    $signName  = '你的短信签名';

    // 短信模板Code https://dysms.console.aliyun.com/dysms.htm?spm=5176.2020520001.1001.3.psXEEJ#/template
    $template_code = '你的短信模版CODE';

    // 短信中的替换变量json字符串
    $json_string_param = '你的短信变量替换字符串';

    // 接收短信的手机号码
    $phone = '接收短信的手机号码';

    // 初始化阿里云config
    AliyunConfig::load();
    // 初始化用户Profile实例
    $profile = DefaultProfile::getProfile("cn-hangzhou", $appKey, $appSecret);
    DefaultProfile::addEndpoint("cn-hangzhou", "cn-hangzhou", "Dysmsapi", "dysmsapi.aliyuncs.com");
    $acsClient = new DefaultAcsClient($profile);
    // 初始化SendSmsRequest实例用于设置发送短信的参数
    $request = new SendSmsRequest();
    // 必填，设置短信接收号码
    $request->setPhoneNumbers($phone);
    // 必填，设置签名名称
    $request->setSignName($signName);
    // 必填，设置模板CODE
    $request->setTemplateCode($template_code);

    // 可选，设置模板参数
    if(!empty($json_string_param)) {
        $request->setTemplateParam($json_string_param);
    }

    // 可选，设置流水号
    // if($outId) {
    //     $request->setOutId($outId);
    // }

    // 发起请求
    $acsResponse =  $acsClient->getAcsResponse($request);
    // 默认返回stdClass，通过返回值的Code属性来判断发送成功与否
    if($acsResponse && strtolower($acsResponse->Code) == 'ok')
    {
        return true;
    }
    return false;