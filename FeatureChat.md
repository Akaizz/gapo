**Feature:** Phần view chatmodule gồm:

- ChatFragment
- ChatDetailFragment

|`	`**ChatFragment**|
| :- |
|initView|Khai báo, khởi tạo View|
|initListener|Xử lý Listener|
|initObserve|Xử lý lắng nghe dữ liệu cập nhật UI từ ChatViewModel|
|initAdapter|Gán adapter tới recyclerView (ở đây là **rcvChatList**)|
|registerEvent|Nơi nhận event mqtt (event: SUPPORT\_CHAT)|
|refreshData|Xử lý hành động làm mới dữ liệu từ remote|
|stopShimmer|Tạm dừng shimmer|
|startShimmer|Bắt đầu shimmer|
|stateLoading|Trạng thái loading khi load dữ liệu từ remote|
|getData|Thực hiện call api getChannels()|
|stateEmpty|Trạng thái empty khi dữ liệu từ Remote bị trống|
|onNewChatMessage|Nhận event mqtt update tin nhắn mới lên UI|
|updateListChannel|Cập nhật trạng thái mới nhất danh sách channel |



Khi User mở chatFragment, ở method initView ta handle các sự kiện: 

- ***registerEvent***: *Lắng nghe và nhận event từ mqttService (event: SUPPORT\_CHAT)*
- ***initAdapter**: khởi tạo adapter và gán adapter tới recyclerView (cụ thể là **rcvChatList**)*

Đồng thời, ta lấy dữ liệu Remote thông qua phương thức **getData.** Response trả về từ ***apiGetListChannel*** sẽ được đổ vào ***chatAdapter*** với phương thức **submitData**. Ở ChatFragment ta cần quan tâm đến 1 vấn đề nữa là việc updateLastMessage của 1 channel từ MQTT, ta lắng nghe event ***MqttEventRegex.SUPPORT\_CHAT*** trong phương thức **registerEvent** và update lên UI last message với channelID tương ứng nhận được từ mqttEvent trả về.



|**ChatDetailFragment**|
| -: |
|initView|Xử lý View|
|initListener|Xử lý Listener|
|initObserve|Xử lý lắng nghe dữ liệu cập nhật UI từ ChatViewModel|
|pickImageWithPermissionCheck|Xin quyền mở thư viện ảnh|
|pickImage|Mở thư viện ảnh|
|onRequestPermissionsResult|Kết quả xin quyền|
|onPermissionsGranted|Quyền truy cập thư viện, máy ảnh được chấp nhận|
|onPermissionsDenied|Quyền truy cập thư viện, máy ảnh bị từ chối|
|setupUI|Trường hợp click outside editText để hide keyboard |
|refreshData|Lấy và làm mới dữ liệu từ Remote|
|getChannelSummary|Lấy thông tin channel detail|
|getMessages|Call API lấy dữ liệu từ remote|
|registerEvent|Lắng nghe event từ mqtt update chatDetail UI|
|onNewChatMessage|Xử lý khi nhận tin nhắn mới từ mqtt event|
|updateAdapter|Cập nhật dữ liêu đổ lên recyclerView|
|chatSendingMessage|Trạng thái sending button sendMessage |
|chatSendMessageFinish|Trạng thái finish button sendMessage|
|chatSendMessageError|Trạng thái Error button sendMessage|
|chatSendAttachError|Trạng thái Error button sendAttach|
|chatSendingAttach|Trạng thái sending button sendAttach|
|chatSendAttachFinish|Trạng thái finish button sendAttach|
|stateLoading|Trạng thái loading data parent view|
|stateError|Trạng thái error data parent view|
|stateEmpty|Trạng thái empty data parent view|


Ở ChatDetailFragment ta tập trung vào xử lý các trạng thái tin nhắn: SENDING, SENT, SENT\_SUCCESS, SENT\_ERROR. Chức năng gửi ảnh chỉ áp dụng với channel Bitu Suppoter.

Khi User mở ChatDetailFragment, ở method initView ta handle các sự kiện: 

- ***registerEvent***: *Lắng nghe và nhận event từ mqttService (event: SUPPORT\_CHAT)*
- ***refreshData***: *Lấy và làm mới dữ liệu từ Remote.*
- ***getChannelSummary:** lấy dữ liệu thông tin channel detail từ remote.*

User tiến hành gửi tin nhắn đến bạn bè, gửi thành công sẽ nhận được 1  ***MqttEventRegex.SUPPORT\_CHAT***  event từ mqttService (Server bắn về cho Client). Ta update messageStatus mới từ event MQTT dựa vào channelID thông qua method **onNewChatMessage(chatMessage) -> cập nhật Adapter.**
