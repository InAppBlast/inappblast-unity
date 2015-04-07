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

For iOS is nesessary to include InAppBlast.framework (our iOS native lib) into generted XCode project.
https://github.com/InAppBlast/inappblast-ios

Next steps are needed for work with iOS push notification.

1. Generate keys for server side.
2. Add statement for get device token and register it within InAppBlast (works on XCode 6 or higher):

		- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
			if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
				// for iOS 8 or higher
				[[UIApplication sharedApplication] registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeSound | UIUserNotificationTypeAlert | UIUserNotificationTypeBadge) categories:nil]];
				[[UIApplication sharedApplication] registerForRemoteNotifications];
			} else {
				// for iOS 7 or lower
				[[UIApplication sharedApplication] registerForRemoteNotificationTypes: (UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
			}

			return YES;
		}

		- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken  {
			[[InAppBlast sharedInstance] addPushDeviceToken:deviceToken];
		}

3. On receive notification:

		- (void) application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
			[[InAppBlast sharedInstance] handleRemoteNotificationWithInfo:userInfo];
		}
