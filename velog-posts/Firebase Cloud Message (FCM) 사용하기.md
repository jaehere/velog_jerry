<h2 id="1-android-클라이언트-설정">1. Android 클라이언트 설정</h2>
<h3 id="fcm-수신하는-서비스-생성">FCM 수신하는 서비스 생성</h3>
<p>FirebaseMessagingService를 상속받은 MyFirebaseMessagingService 클래스 생성</p>
<pre><code>
import android.app.NotificationChannel
import android.app.NotificationManager
import androidx.core.app.NotificationCompat
import com.google.firebase.messaging.FirebaseMessagingService
import com.google.firebase.messaging.RemoteMessage

class MyFirebaseMessagingService() : FirebaseMessagingService() {

    override fun onMessageReceived(message: RemoteMessage) {
        super.onMessageReceived(message)

        // Create the NotificationChannel.
        val name = "채팅 알림"
        val descriptionText = "채팅 알림입니다."
        val importance = NotificationManager.IMPORTANCE_DEFAULT
        val mChannel = NotificationChannel(getString(R.string.default_notification_channel_id), name, importance)
        mChannel.description = descriptionText
        // Register the channel with the system. You can't change the importance
        // or other notification behaviors after this.
        val notificationManager = getSystemService(NOTIFICATION_SERVICE) as NotificationManager
        notificationManager.createNotificationChannel(mChannel)

        val body = message.notification?.body ?: ""
        val notificationBuilder = NotificationCompat.Builder(applicationContext, getString(R.string.default_notification_channel_id))
            .setSmallIcon(R.drawable.baseline_chat_24)
            .setContentTitle(getString(R.string.app_name))
            .setContentText(body)

        notificationManager.notify(0, notificationBuilder.build())
    }


    override fun onNewToken(token: String) {
        super.onNewToken(token)
    }
}</code></pre><h3 id="앱-매니페스트-수정">앱 매니페스트 수정</h3>
<p>FirebaseMessagingService를 확장하는 서비스를 추가</p>
<pre><code>&lt;service
    android:name=".MyFirebaseMessagingService"
    android:exported="false"&gt;
    &lt;intent-filter&gt;
        &lt;action android:name="com.google.firebase.MESSAGING_EVENT" /&gt;
    &lt;/intent-filter&gt;
&lt;/service&gt;
</code></pre><h3 id="선택사항-android80api-수준-26-이상부터는-알림-채널이-지원-및-권장된다">(선택사항) Android8.0(API 수준 26) 이상부터는 <a href="https://developer.android.com/guide/topics/ui/notifiers/notifications.html?hl=ko#ManageChannels">알림 채널</a>이 지원 및 권장된다.</h3>
<pre><code>&lt;meta-data
    android:name="com.google.firebase.messaging.default_notification_channel_id"
    android:value="@string/default_notification_channel_id" /&gt;</code></pre><h3 id="android-13-이상에서-런타임-알림-권한-요청">Android 13 이상에서 런타임 알림 권한 요청</h3>
<p>onCreate 함수에서 askNotificationPermission() 호출</p>
<pre><code>    private fun askNotificationPermission() {
        // This is only necessary for API level &gt;= 33 (TIRAMISU)
        if (Build.VERSION.SDK_INT &gt;= Build.VERSION_CODES.TIRAMISU) {
            if (ContextCompat.checkSelfPermission(this, POST_NOTIFICATIONS) ==
                PackageManager.PERMISSION_GRANTED
            ) {
                // FCM SDK (and your app) can post notifications.
            } else if (shouldShowRequestPermissionRationale(POST_NOTIFICATIONS)) {
                showPermissionRationalDialog()
            } else {
                // Directly ask for the permission
                requestPermissionLauncher.launch(POST_NOTIFICATIONS)
            }
        }
    }

    @RequiresApi(Build.VERSION_CODES.TIRAMISU)
    private fun showPermissionRationalDialog() {
        AlertDialog.Builder(this)
            .setMessage("알림 권한이 없으면 알림을 받을 수 없습니다.")
            .setPositiveButton("권한 허용하기") { _, _ -&gt;
                requestPermissionLauncher.launch(POST_NOTIFICATIONS)
            }.setNeutralButton("취소") { dialogInterface, _ -&gt;
                dialogInterface.cancel()
            }.show()
    }
</code></pre><h3 id="등록된-token-확인하기">등록된 token 확인하기</h3>
<pre><code>Firebase.messaging.token.addOnCompleteListener {
    val token = it.result
}</code></pre><hr />
<h2 id="2-push-테스트">2. Push 테스트</h2>
<h3 id="firebase-console-테스트">Firebase Console 테스트</h3>
<p>1) 콘솔 &gt; 프로젝트 선택 &gt; 왼쪽 바에서 "실행" &gt; Messaging &gt; 첫번째 캠페인 &gt; Friebase 메시지 온보딩 &gt; Firebase 알림 메시지
<img alt="" src="https://velog.velcdn.com/images/jaehere/post/bf006430-ba80-43ab-b9ff-36f78c46bb69/image.png" /></p>
<p>2) 테스트 메시지 전송
<img alt="" src="https://velog.velcdn.com/images/jaehere/post/bc9c3b21-60ef-4e60-b88b-d0bac1880901/image.png" /></p>
<p>3) FCM 토큰 입력 후 테스트 버튼 클릭
<img alt="" src="https://velog.velcdn.com/images/jaehere/post/79fccbcd-a480-4e15-8f3e-cd2911eabb04/image.png" /></p>
<p>4) 기기 상에서 알림 확인
<img alt="" src="https://velog.velcdn.com/images/jaehere/post/22d5c8a1-8236-4a5e-94e9-1c06749b4e7b/image.png" /></p>
<h3 id="postmanhttp-테스트">Postman(HTTP) 테스트</h3>
<p>Firebase Cloud Messaging API(V1) <a href="https://firebase.google.com/docs/cloud-messaging/migrate-v1?hl=ko&amp;authuser=0&amp;_gl=1*hk5ra9*_ga*NTg4OTE3MzY2LjE3NDE1NzA3MjU.*_ga_CW55HF8NVT*MTc0MTY2MjAzNi43LjEuMTc0MTY2MjYwNC41Ni4wLjA.">(관련 문서 링크)</a></p>
<p>1) 서버 엔드포인트 업데이트 (POST 방식)
<code>https://fcm.googleapis.com/v1/projects/Firebase프로젝트ID/messages:send</code></p>
<p><img alt="" src="https://velog.velcdn.com/images/jaehere/post/1d15729e-e632-4d0d-af96-b74f9b4f1856/image.png" /></p>
<p> Firebase 프로젝트 ID는 Firebase 콘솔의 일반 프로젝트 설정 탭에서 이 ID를 확인할 수 있다.</p>
<p>2) Authorization
Authorize 버튼을 클릭 -  구글 계정으로 로그인 - 
"Firebase 애플리케이션 메시지 보내기 및 메시지 구독 관리" 허용 하여 토큰을 얻어온다.
<img alt="" src="https://velog.velcdn.com/images/jaehere/post/9be55487-1055-4127-b194-defb37270ad3/image.png" /></p>
<p>3) Body &gt; raw 창에 FCM TOKEN 정보 및 테스트 메시지를 입력 후 Send 버튼을 누른다.</p>
<pre><code>{
   "message":{
      "token":"${FCM_TOKEN}",
      "notification":{
        "body":"안녕",
        "title":"테스트"
      }
   }
}</code></pre><p>4)  기기 상에서 알림 확인
<img alt="" src="https://velog.velcdn.com/images/jaehere/post/dd7ab9c2-22e4-40e8-b52a-840674a7db72/image.png" /></p>