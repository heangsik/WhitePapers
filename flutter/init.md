# Flutter 시작

## 설치

## 프로젝트

### 생성

- flutter create project_name

### GetX

#### 설치

- flutter pub add get

### 사용법

- stor.dart 파일 생성

  ```dart
  // store.dart
  import 'package:get/get.dart';

  class Store extends GetxController {
  static Store get to => Get.find();

  String serverIp = '';
  String userName = '';

  void updateUserName(String value) {
      userName = value;
  }

  final messages = [].obs;

  void addMessage(String value) {
      print('rx message send');
      messages.add(value);
  }
  }
  ```

- bind.dart 파일 생성

  ```dart
  import 'package:flutter_chat_client/store/Store.dart';
  import 'package:get/get.dart';

  class InitBinding extends Bindings {
  @override
  void dependencies() {
      Get.put(Store(), permanent: true);
  }
  }

  ```

- main.dart 수정

  ```dart
  import 'package:flutter/material.dart';
  import 'package:flutter_chat_client/store/init_binding.dart';
  import 'package:flutter_chat_client/widget/chat_page_rx.dart';
  import 'package:flutter_chat_client/widget/connection_page.dart';
  import 'package:flutter_chat_client/widget/store_test.dart';
  import 'package:get/get.dart';
  import 'widget/chat_page.dart';

  void main() {
  runApp(const MyApp());
  }

  class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
      return GetMaterialApp( // <---- 변경
      title: 'Flutter Chat',
      theme: ThemeData(
          colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
          useMaterial3: true,
      ),
      initialRoute: '/connect',
      getPages: [
          GetPage(name: '/connect', page: () => const ConnectionPage()),
          GetPage(name: '/store', page: () => const StoreWidget()),
          GetPage(name: '/chat', page: () => const ChatPage(title: 'Chat Main')),
          GetPage(
              name: '/chat_rx', page: () => const ChatPageRx(title: 'Chat Main')),
      ],
      initialBinding: InitBinding(), // <-- 추가
      );
  }
  }

  ```

- 사용법

  ```dart
  Store.to.updateUserName(value);
  Stroe.to.userName;
  ```
