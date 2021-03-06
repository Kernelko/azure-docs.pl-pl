---
title: Wysyłanie powiadomień push do aplikacji systemu Android przy użyciu usług Azure Notification Hubs i Firebase Cloud Messaging | Microsoft Docs
description: Korzystając z tego samouczka, dowiesz się, jak za pomocą usług Azure Notification Hubs i Google Firebase Cloud Messaging wysyłać powiadomienia push do urządzeń z systemem Android.
services: notification-hubs
documentationcenter: android
keywords: powiadomienia wypychane, powiadomienie wypychane, powiadomienia wypychane w systemie android, firebase cloud messaging
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 2bc085989ff3bbbc50042c46b338f748a10aa87e
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38232800"
---
# <a name="tutorial-push-notifications-to-android-devices-by-using-azure-notification-hubs-and-google-firebase-cloud-messaging"></a>Samouczek: wysyłanie powiadomień push do urządzeń z systemem Android przy użyciu usługi Azure Notification Hubs i Google Firebase Cloud Messaging
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

> [!IMPORTANT]
> W tym artykule przedstawiono powiadomienia push korzystające z usługi Google Firebase Cloud Messaging (FCM). Jeśli nadal używasz usługi Google Cloud Messaging (GCM), zobacz [Wysyłanie powiadomień push do urządzeń z systemem Android przy użyciu usług Azure Notification Hubs i GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).

Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia push do aplikacji dla systemu Android przy użyciu usług Azure Notification Hubs i Firebase Cloud Messaging. W tym samouczku tworzysz pustą aplikację dla systemu Android służącą do odbierania powiadomień push przy użyciu usługi Firebase Cloud Messaging (FCM).

Kompletny kod dla tego samouczka można pobrać z witryny GitHub [tutaj](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).

W tym samouczku wykonasz następujące kroki:

> [!div class="checklist"]
> * Tworzenie projektu programu Android Studio.
> * Tworzenie projektu Firebase obsługującego usługę Firebase Cloud Messaging.
> * Tworzenie centrum powiadomień.
> * Łączenie aplikacji z centrum powiadomień.
> * Testowanie aplikacji. 

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków tego samouczka potrzebne jest aktywne konto platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/free/).

* Oprócz aktywnego konta platformy Azure wspomnianego powyżej w tym samouczku wymagana jest najnowsza wersja programu [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).
* System Android 2.3 lub nowszy dla usługi Firebase Cloud Messaging.
* Usługa Google Repository w wersji 27 lub nowszej jest wymagana dla usługi Firebase Cloud Messaging.
* Usługi Google Play Services 9.0.2 lub nowsze dla usługi Firebase Cloud Messaging.
* Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usługi Notification Hubs dotyczących aplikacji dla systemu Android.

## <a name="create-an-android-studio-project"></a>Tworzenie projektu programu Android Studio
1. W programie Android Studio utwórz nowy projekt programu Android Studio.
   
    ![Android Studio — nowy projekt](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)
2. Wybierz kształt **Phone and Tablet** (Telefon i tablet) oraz odpowiednią wersję z listy **Minimum SDK** (Minimalna wersja zestawu SDK), która ma być obsługiwana. Następnie kliknij przycisk **Next** (Dalej).
   
    ![Android Studio — przepływ pracy tworzenia projektu](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)
3. Wybierz pozycję **Empty Activity** (Puste działanie) dla głównego działania i kliknij przycisk **Next** (Dalej), a następnie kliknij przycisk **Finish** (Zakończ).

## <a name="create-a-firebase-project-that-supports-fcm"></a>Tworzenie projektu Firebase obsługującego usługę FCM
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Konfigurowanie centrum powiadomień
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-firebase-cloud-messaging-settings-for-the-hub"></a>Konfigurowanie ustawień usługi Firebase Cloud Messaging na potrzeby centrum
1. Wybierz opcję **Google (GCM)** w kategorii **USTAWIENIA POWIADOMIEŃ**. 
2. Wprowadź klucz interfejsu API (klucz serwera FCM) skopiowany wcześniej z [konsoli Firebase](https://firebase.google.com/console/).
3. Wybierz pozycję **Zapisz** na pasku narzędzi.

    ![Azure Notification Hubs — Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

Twoje centrum powiadomień jest teraz skonfigurowane do pracy z usługą Firebase Cloud Messaging i uzyskano parametry połączenia do rejestrowania aplikacji w celu odbierania i wysyłania powiadomień push.

## <a id="connecting-app"></a>Łączenie aplikacji z centrum powiadomień
### <a name="add-google-play-services-to-the-project"></a>Dodawanie usług Google Play do projektu
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Dodawanie bibliotek usługi Azure Notification Hubs
1. W pliku `Build.Gradle` dla **aplikacji** dodaj następujące wiersze w sekcji **dependencies**.
   
    ```java
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```

2. Dodaj następujące repozytorium po sekcji **dependencies**.
   
    ```java
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }
    ```

### <a name="add-google-firebase-support"></a>Dodawanie obsługi usługi Google Firebase

1. W pliku `Build.Gradle` dla **aplikacji** dodaj następujące wiersze w sekcji **dependencies**.
   
    ```java
        compile 'com.google.firebase:firebase-core:12.0.0'
    ```

2. Na końcu pliku dodaj następującą wtyczkę. 
   
    ```java
    apply plugin: 'com.google.gms.google-services'
    ```

### <a name="updating-the-androidmanifestxml"></a>Aktualizowanie pliku AndroidManifest.xml
1. W celu zapewnienia obsługi usługi FCM musisz zaimplementować w naszym kodzie wystąpienie usługi odbiornika interfejsu Instance ID, który służy do [uzyskiwania tokenów rejestracji](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) przy użyciu [interfejsu API FirebaseInstanceId firmy Google](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId). W tym samouczku nazwa klasy to `MyInstanceIDService`. 
   
    Dodaj następującą definicję usługi do pliku AndroidManifest.xml wewnątrz tagu `<application>`. 
   
    ```xml
        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>
    ```

2. Po uzyskaniu tokenu rejestracji usługi FCM z interfejsu API FirebaseInstanceId używa się go do [rejestracji w usłudze Azure Notification Hubs](notification-hubs-push-notification-registration-management.md). Zapewniasz obsługę rejestracji w tle przy użyciu klasy `IntentService` o nazwie `RegistrationIntentService`. Ta usługa odpowiada również za odświeżanie Twojego tokenu rejestracji usługi FCM.
   
    Dodaj następującą definicję usługi do pliku AndroidManifest.xml wewnątrz tagu `<application>`. 
   
    ```xml
        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>
    ```

3. Aby otrzymywać powiadomienia, musisz również zdefiniować odbiornik. Dodaj następującą definicję odbiornika do pliku AndroidManifest.xml wewnątrz tagu `<application>`. Zastąp symbol zastępczy `<your package>` rzeczywistą nazwą pakietu wyświetlaną u góry pliku `AndroidManifest.xml`.

    ```xml
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
    ```

4. Dodaj następujące wymagane uprawnienia powiązane z usługą FCM poniżej tagu `</application>`. 
   
    Aby uzyskać więcej informacji na temat tych uprawnień, zobacz [Setup a GCM Client app for Android](https://developers.google.com/cloud-messaging/android/client#manifest) (Konfigurowanie aplikacji klienckiej usługi GCM dla systemu Android) i [Migrate a GCM Client App for Android to Firebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm) (Migrowanie aplikacji klienckiej usługi GCM dla systemu Android do usługi Firebase Cloud Messaging).
   
    ```xml
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```

### <a name="adding-code"></a>Dodawanie kodu
1. W widoku projektu rozwiń węzeł **app** > **src** > **main** > **java**. Kliknij prawym przyciskiem folder pakietu w węźle **java**, kliknij pozycję **New** (Nowy), a następnie kliknij pozycję **Java Class** (Klasa Java). Dodaj nową klasę o nazwie `NotificationSettings`. 
   
    ![Android Studio – nowa klasa Java](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)
   
    Zaktualizuj te trzy symbole zastępcze w poniższym kodzie dla klasy `NotificationSettings`:
   
   * **SenderId**: identyfikator nadawcy uzyskany wcześniej na karcie **Cloud Messaging** w ustawieniach projektu w [konsoli Firebase](https://firebase.google.com/console/).
   * **HubListenConnectionString**: domyślne parametry połączenia **DefaultListenAccessSignature** dla centrum. Możesz skopiować te parametry połączenia, klikając pozycję **Zasady dostępu** w centrum w witrynie [Azure Portal].
   * **HubName**: użyj nazwy centrum powiadomień wyświetlanej na stronie centrum w witrynie [Azure Portal].
     
     `NotificationSettings` — kod:
     
    ```java
       public class NotificationSettings {
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
       }
    ```

2. Przy użyciu poprzednich kroków dodaj nową klasę o nazwie `MyInstanceIDService`. Ta klasa to Twoja implementacja usługi odbiornika interfejsu Instance ID.
   
    Kod dla tej klasy będzie wywoływać klasę `IntentService` w celu [odświeżenia tokenu usługi FCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) w tle.
   
    ```java
        import android.content.Intent;
        import android.util.Log;
        import com.google.firebase.iid.FirebaseInstanceIdService;

        public class MyInstanceIDService extends FirebaseInstanceIdService {

            private static final String TAG = "MyInstanceIDService";

            @Override
            public void onTokenRefresh() {

                Log.d(TAG, "Refreshing GCM Registration Token");

                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };
    ```

1. Dodaj kolejną nową klasę o nazwie `RegistrationIntentService` do projektu. Ta klasa implementuje interfejs `IntentService` oraz obsługuje [odświeżanie tokenu usługi FCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) i [rejestrowanie w centrum powiadomień](notification-hubs-push-notification-registration-management.md).
   
    Użyj następującego kodu dla tej klasy.
   
    ```java
        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;        
        import com.google.firebase.iid.FirebaseInstanceId;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class RegistrationIntentService extends IntentService {
   
            private static final String TAG = "RegIntentService";
   
            private NotificationHub hub;
   
            public RegistrationIntentService() {
                super(TAG);
            }
   
            @Override
            protected void onHandleIntent(Intent intent) {
   
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
                String storedToken = null;
   
                try {
                    String FCM_token = FirebaseInstanceId.getInstance().getToken();
                    Log.d(TAG, "FCM Registration Token: " + FCM_token);
   
                    // Storing the registration ID that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    // Check if the token may have been compromised and needs refreshing.
                    else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete registration", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
   
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }
    ```

2. W klasie `MainActivity` dodaj następujące instrukcje `import` nad deklaracją klasy.
   
    ```java
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
    ```
3. Dodaj następujące prywatne składowe u góry klasy. Pola służą do [sprawdzania dostępności usług Google Play zgodnie z zaleceniami firmy Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).
   
    ```java
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
    ```

4. W klasie `MainActivity` dodaj następującą metodę do sprawdzania dostępności usług Google Play. 
   
    ```java
        /**
        * Check the device to make sure it has the Google Play Services APK. If
        * it doesn't, display a dialog that allows users to download the APK from
        * the Google Play Store or enable it in the device's system settings.
        */
    
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }
    ```

5. W klasie `MainActivity` dodaj następujący kod, który sprawdza dostępność usług Google Play przed wywołaniem klasy `IntentService` w celu pobrania tokenu rejestracji FCM i rejestracji w centrum powiadomień.
   
    ```java
        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService to register this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
    ```

6. W metodzie `OnCreate` klasy `MainActivity` dodaj następujący kod w celu uruchomienia procesu rejestracji po utworzeniu działania.
   
    ```java
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
    ```

7. Aby zweryfikować stan aplikacji i zgłosić stan w aplikacji, dodaj te dodatkowe metody do klasy `MainActivity`.
   
    ```java
        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
   
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
   
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
   
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
   
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }
    ```

8. Metoda `ToastNotify` używa kontrolki *"Hello World"* `TextView` do trwałego zgłaszania stanu i powiadomień w aplikacji. W układzie activity_main.xml dodaj następujący identyfikator dla tej kontrolki.
   
    ```java
       android:id="@+id/text_hello"
    ```

9. Następnie dodaj podklasę dla odbiornika zdefiniowanego w pliku AndroidManifest.xml. Dodaj kolejną nową klasę o nazwie `MyHandler` do projektu.
10. Dodaj następujące instrukcje import u góry pliku `MyHandler.java`:
    
    ```java
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
    ```

11. Dodaj następujący kod do klasy `MyHandler`, aby stała się podklasą klasy `com.microsoft.windowsazure.notifications.NotificationsHandler`.
    
    Ten kod zastępuje metodę `OnReceive`, dlatego program obsługi zgłasza odbierane powiadomienia. Program obsługi wysyła również powiadomienie wypychane do menedżera powiadomień systemu Android za pomocą metody `sendNotification()`. Metoda `sendNotification()` powinna być wykonywana, gdy aplikacja nie jest uruchomiona i otrzymano powiadomienie.
    
    ```java
        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
    
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
    
            private void sendNotification(String msg) {
    
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
    
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
    
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
    
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
    
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }
    ```

12. W programie Android Studio na pasku menu kliknij pozycję **Build** > **Rebuild Project** (Kompiluj — Kompiluj ponownie projekt), aby upewnić się, że w kodzie nie występują żadne błędy.
13. Uruchom aplikację na urządzeniu i sprawdź, czy pomyślnie rejestruje się ona w centrum powiadomień. 
    
    > [!NOTE]
    > Rejestracja podczas początkowego uruchamiania może kończyć się niepowodzeniem, dopóki nie zostanie wywołana metoda `onTokenRefresh()` usługi identyfikatora wystąpienia. Odświeżenie powinno zainicjować pomyślną rejestrację w centrum powiadomień.
    > 
    > 

## <a name="test-the-app"></a>Testowanie aplikacji
### <a name="test-send-notification-from-the-notification-hub"></a>Wysyłanie testowego powiadomienia z centrum powiadomień
Powiadomienia push możesz wysłać z witryny [Azure Portal], wykonując następujące czynności: 

1. Wybierz opcję **Wysyłanie testowe** w sekcji **Rozwiązywanie problemów**.
2. W pozycji **Platformy** wybierz opcję **Android**. 
3. Wybierz pozycję **Wyślij**.  Nie zobaczysz powiadomienia na urządzeniu z systemem Android, ponieważ jeszcze nie uruchomiono na nim aplikacji mobilnej. Po uruchomieniu aplikacji mobilnej wybierz przycisk **Wyślij** ponownie, aby zobaczyć komunikat powiadomienia. 
4. Zobacz **wynik** operacji na liście u dołu. 

    ![Azure Notification Hubs — Wysyłanie testowe](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)


[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


### <a name="run-the-mobile-app"></a>Uruchamianie aplikacji mobilnej
Aby przetestować powiadomienia wypychane w emulatorze, upewnij się, że obraz emulatora obsługuje poziom interfejsu API Google wybrany dla aplikacji. Jeśli obraz nie obsługuje natywnych interfejsów API Google, wystąpi wyjątek **SERVICE\_NOT\_AVAILABLE** (usługa niedostępna).

Oprócz tego upewnij się, że dodano konto Google do uruchomionego emulatora w obszarze **Settings**  >  **Accounts** (Ustawienia > Konta). W przeciwnym razie próby rejestracji w usłudze GCM mogą spowodować wystąpienie wyjątku **AUTHENTICATION\_FAILED** (uwierzytelnianie nie powiodło się).

1. Uruchom aplikację i zwróć uwagę, czy identyfikator rejestracji jest zgłaszany, co oznacza pomyślną rejestrację.
   
    ![Testowanie w systemie Android — rejestracja kanału](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)
2. Wprowadź komunikat powiadomienia, który ma zostać wysłany do wszystkich urządzeń z systemem Android zarejestrowanych w centrum.
   
    ![Testowanie w systemie Android — wysyłanie komunikatu](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)
3. Naciśnij przycisk **Send Notification** (Wyślij powiadomienie). Na wszystkich urządzeniach, na których jest uruchomiona aplikacja, jest wyświetlane wystąpienie kontrolki `AlertDialog` z komunikatem powiadomienia push. Urządzenia, na których aplikacja nie jest uruchomiona, ale zostały wcześniej zarejestrowane do otrzymywania powiadomień push, otrzymują powiadomienie w menedżerze powiadomień systemu Android. Powiadomienia można wyświetlić, szybko przesuwając w dół od lewego górnego rogu.
   
    ![Testowanie w systemie Android — powiadomienia](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

## <a name="next-steps"></a>Następne kroki
W tym samouczku za pomocą usługi Firebase Cloud Messaging wysłano powiadomienia push do urządzeń z systemem Android. Aby dowiedzieć się, jak wypychać powiadomienia przy użyciu usługi Google Cloud Messaging, przejdź do następującego samouczka: 

> [!div class="nextstepaction"]
>[Wysyłanie powiadomień push do urządzeń z systemem Android przy użyciu usługi Google Cloud Messaging](notification-hubs-android-push-notification-google-gcm-get-started.md)


<!-- Images. -->


<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Notification Hubs Guidance]: notification-hubs-push-notification-overview.md
[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure Portal]: https://portal.azure.com
