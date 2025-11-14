EdgeDetect — Real-Time Image Edge Detection (OpenGL ES + OpenCV + JNI)

This project implements a real-time edge detection pipeline on Android using:

OpenGL ES 2.0 (GPU rendering)

C++ / JNI (native processing)

OpenCV (image operations)

Android Camera2 (optional extension)

The app loads a sample image, runs it through a native C++ edge-detection function using OpenCV (Sobel/Canny), and displays output using a custom GLSurfaceView + GLSurfaceView.Renderer.

You can toggle between Raw Image and Edge Output using a button in the UI.

Features
✅ GPU Rendering – OpenGL ES 2.0

Renders textures at high performance and displays both raw & processed frames.

✅ Native C++ Processing

JNI bridge calls a native function:

processFrame(uint8_t* input, int width, int height)


Uses OpenCV for:

Grayscale conversion

Gaussian blur

Canny/Sobel edges

Returning processed frame back to Kotlin.

✅ Proper NDK + CMake Integration

Supports all major ABIs:

arm64-v8a

armeabi-v7a

x86

x86_64

✅ OpenCV Android Integration

Includes:

libopencv_java4.so

OpenCV header linking

Correct OpenCV_DIR supplied via CMake

Required libc++_shared.so bundled

✅ Clean UI

Simple GLSurfaceView for live preview

Toggle button for switching modes

FPS counter (updated in UI thread)

Project Structure
app/
  src/
    main/
      java/com/example/edgedetect/
          MainActivity.kt
          GLRenderer.kt
          NativeBridge.kt
      cpp/
          native-lib.cpp
          CMakeLists.txt
      res/
          layout/activity_main.xml
          drawable/sample.png
      jniLibs/
          arm64-v8a/
              libc++_shared.so
              libopencv_java4.so
          armeabi-v7a/ ...
          x86/ ...
          x86_64/ ...

Native Pipeline (C++)
1. Input ByteArray → Mat
cv::Mat src(height, width, CV_8UC4, inputData);

2. Process Image
cv::cvtColor(src, gray, cv::COLOR_RGBA2GRAY);
cv::GaussianBlur(gray, blur, cv::Size(5,5), 0);
cv::Canny(blur, edges, 80, 150);

3. Convert Back to RGBA
cv::cvtColor(edges, rgbaOut, cv::COLOR_GRAY2RGBA);

4. Return to Kotlin as ByteArray
How to Build
1. Clone Repository
git clone https://github.com/<your-username>/EdgeDetect.git

2. Open in Android Studio

File → Open → Select project folder

Let Gradle sync

Ensure NDK + CMake are installed

3. Run on real device

Connect Android device

Press Run ▶️

Requirements

Android Studio Ladybug or later

Android 7.0+

NDK 26+

CMake 3.22+

OpenCV 4.x Android SDK

Known Issues
❗ If app crashes with

dlopen failed: library "libc++_shared.so" not found
You must include:

app/src/main/jniLibs/<ABI>/libc++_shared.so

❗ If OpenCV headers not found

Check CMake include_directories() and correct OpenCV path.

Future Extensions

Live camera edge detection (Camera2 → SurfaceTexture → GL)

Real-time video edge pipeline with OpenGL FBO

Running Sobel/Canny fully on GPU

Adding more filters (Emboss, Sharpen, Cartoonify, Pencil Sketch)

License

This project is for educational and interview-assignment purposes.
You may reuse or extend the code with attribution.
