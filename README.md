## Tutorial

Install in-app-blast.unitypackage

Add object to scene: Assets\InAppBlast\InAppBlast.prefab

Initialize InAppBlast service in any convenient MonoBehaviour: 

		void Start ()
		{
			// ...
			InAppBlast.Init("<project_key>", InAppBlastLogLevel.InAppBlastLogLevelAll, OnInAppBlastInit);
			// ...
		}

		void OnInAppBlastInit()
		{
			// service is successfully initialized and ready to work!
			InAppBlast.RegisterUser("<user_name>");
		}

For iOS is nesessary to include InAppBlast.framework (our iOS lib ) into generted XCode project.
https://github.com/InAppBlast/inappblast-ios

Next steps are needed for work with iOS push notification.

1. сгенерировать ключи для сервера
2. добавить код по получению идентификатора устройства и регистрации его на сервере InAppBlast:

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

3. On receive notification:

		- (void) application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
			[[InAppBlast sharedInstance] handleRemoteNotificationWithInfo:userInfo];
		}
