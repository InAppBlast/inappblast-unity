## Tutorial

Установите in-app-blast.unitypackage

Добавить в сцену объект: Assets\InAppBlast\InAppBlast.prefab

Инициализировать сервис в любом удобном MonoBehaviour: 

void Start ()
{
	// ...
InAppBlast.Init("<project_key>", InAppBlastLogLevel.InAppBlastLogLevelAll, OnInAppBlastInit);
	// ...
}

void OnInAppBlastInit()
{
	// сервис успешно инициализирован и готов к работе
InAppBlast.RegisterUser("<user_name>");
}

Для iOS дополнительно необходимо добавить в xcode проект фреймворк InAppBlast.framework

Для пуш нотификаций:
сгенерировать ключи для сервера
добавить код по получению идентификатора устройства и регистрации его на сервере InAppBlast:
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

   // Для iOS 7 и ниже
   [[UIApplication sharedApplication] registerForRemoteNotificationTypes: (UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];

   // Для iOS 8 и выше
   // [[UIApplication sharedApplication] registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeSound | UIUserNotificationTypeAlert | UIUserNotificationTypeBadge) categories:nil]];
   // [[UIApplication sharedApplication] registerForRemoteNotifications];

   return YES;
}

- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken  {
   [[InAppBlast sharedInstance] addPushDeviceToken:deviceToken];
}

получение пуш нотификации:

- (void) application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
  [[InAppBlast sharedInstance] handleRemoteNotificationWithInfo:userInfo];
}
