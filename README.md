### xash3d old - all archive
# 2017
EN:
[Building in 2017](#Building2017)
[Manual running in 2017](#Manual-running2017)

RU:
[Компилирование 2017 г.](#buildru2017)
[Руководство и запуск 2017 г.](#руководство2017)













# Building2017
# CMake (recommended)

If you want to enable VGUI support, you need part of Half-Life 1 SDK to build vgui_support library.

**NOTE**: VGUI is exists only for Linux/MacOSX/Windows and only for x86. Disable it by -DXASH_VGUI=no if you are targetting non-supported architecture.

**Clone Xash3D repository using git:**

```git clone --recursive https://github.com/FWGS/xash3d```

**Move to the Xash3D folder:**

```cd xash3d```

**Clone Half Life 1 SDK with git:**

```git clone https://github.com/ValveSoftware/halflife hlsdk/```

**OR** you can use vgui-dev repository with just ripped off VGUI headers and library. Clone it using git:

```git clone https://github.com/FWGS/vgui-dev hlsdk/```

**Implying VGUI headers and library is cloned into hlsdk you should be able to build Xash3D as:**

```
mkdir -p build
cd build
```

**On 32 bit systems:**

```cmake -DHL_SDK_DIR=../hlsdk -DXASH_SDL=yes -DXASH_VGUI=yes ../ && make```

**On 64 bit systems:**

```cmake -DHL_SDK_DIR=../hlsdk -DXASH_SDL=yes -DXASH_VGUI=yes -DCMAKE_C_FLAGS="-m32" -DCMAKE_CXX_FLAGS="-m32" -DCMAKE_EXE_LINKER_FLAGS="-m32" ../ && make```

**NOTE: On Windows you need to pass additional keys to target specific version of Visual Studio and set SDL2 path. For example:**

```-G "Visual Studio 15 2017" -DSDL2_PATH="c:\projects\sdl2\"```

**to build with Visual Studio 2017. See CMake documentation for more info. You can download SDL2 development files at official site.**

**NOTE: Xash3D FWGS itself supports 64-bit build, but due to it's useless, it's not allowed by default. If you really know WHY you need 64 bit build of Xash3D FWGS, pass -DXASH_64BIT=yes to cmake.**

See other possible build configurations in our wiki: Possible-configurations
Manual build (without CMake)
Building on Windows
Modern Visual Studio

There is a solution for VS2017 in msvc/ dir in the root directory of repository. At least should be.
Visual Studio 6

This is legacy configuration, but MSVC6 seems to generate more stable and more effective code

**Setup your msvc6 enviroment, unpack SDL2 to xash3d folder and do:**

```
cd (xash3d)\engine
..\msvc6\build.bat
cd (xash3d)\game_launch
..\msvc6\build_launch.bat
```

Building game mods
Linux
microndk

**All mods that ported to android may be build to linux using Android.mk with microndk:**

```[https://github.com/SDLash3D/microndk]```

Clone microndk repo somewhere, change xash3d_config to preffered configuration (change arch to x86 for example)

**Go to dlls folder if you are building server or cl_dlls if you are building client and do:**

```make -f /path/to/microndk/microndk.mk -j4```

**Do:**

```make -f /path/to/microndk/microndk.mk -j4 clean```

every time when you build client after building server
Makefile.linux
Windows

On windows common way is using Visual Studio as many mods does not correctly work with mingw.

Just open project and build it.

Other is using mingw and microndk, but it was not tested yet.

hlsdk-xash3d based mods may work fine with mingw.

You may use microndk to build it. Build process is very similar to linux one.

**After setting up MinGW enviroment, do:**
```mingw32-make -f \path\to\microndk\Microndk.mk```

**to build 64-bit library, use:**
```mingw32-make -f \path\to\microndk\Microndk.mk 64BIT=1```

Edit xash3d_config.mk to set optimal CFLAGS if you are running server
Running

**Copy valve folder from Half-Life:**

```cp -r $HOME/.steam/steam/steamapps/common/Half-Life/valve $HOME/Games/Xash3D```

**NOTE: If you build with CMake, you can now use make install. It will install binaries where they must be located. So just run xash3d from command line in directory where is gamedata is located. For additional info look to the CMakeLists.txt in repo root and xash3d.sh script.**

**After a successful build, copy the next files to some other directory where you want to run Xash3D:**

```cp engine/libxash.so game_launch/xash3d mainui/libxashmenu.so $HOME/Games/Xash3D```

**If you built it with XASH_VGUI, also copy vgui.so:**

```cp ../hlsdk/linux/vgui.so vgui_support/libvgui_support.so $HOME/Games/Xash3D```

**Copy script:**

```cp ../xash3d.sh $HOME/Games/Xash3D```

**Run:**

```
cd $HOME/Games/Xash3D
./xash3d.sh
```


# Manual-running2017

**Put xash3d binaries and vgui(optionally) to you game data directory and run:**

```LD_LIBRARY_PATH=. ./xash -dev 5```

Manual Running on Windows

After the build has completed, copy the following files into a directory of your choice:

# Built by the compile process:
build/game_launch/[Build Config]/xash.exe
build/engine/[Build Config]/xash_sdl.dll
build/mainui/[Build Config]/menu.dll
build/vgui_support/[Build Config]/vgui_support.dll

# Automatically downloaded dependencies:
build/SDL2/[SDL version]/lib/x86/SDL2.dll
build/VGUI/lib/win32_vc6/vgui.dll

# From your Steam games directory:
Half-Life/valve

**Running under GDB**

```LD_LIBRARY_PATH=. gdb --args ./xash -dev 5```

Using DLL Loader

**Put vgui_support.dll from windows build to your game data folder and run:**

```LD_LIBRARY_PATH=. ./xash -dev 5 -vguilib vgui.dll -clientlib valve/cl_dlls/client.dll -dll dlls/hl.dll```

Running MinGW builds

**Put exe file to your game data directory**

```
cd (game)\
xash_bin -dev 5
```

**Running MinGW builds under GDB**

```gdb --args ./xash_bin.exe -dev 5```

Useful GDB commands

**Start or restart process:**

```(gdb) r```

**Show backtrace:**

```(gdb) bt```

**Full backtrace:**

```(gdb) bt full```

**Continue execution:**

```(gdb) c```

**Recover from segmentation fault:**

**Skipping function:**

```
(gdb) handle SIGSEGV nopass
(gdb) ret
(gdb) c
```

**Restart frame for beginning:**
```
(gdb) handle SIGSEGV nopass
(gdb) jump *Host_AbortCurrentFrame
```

**Anti-Crash script (useful for scripting servers):**

```
cd /path/to/rootdir
file xash
set args xash -dev 5 -nowcon
handle SIGSEGV nopass
catch signal SIGSEGV
set $crashes=0
commands
print $crashes++
if $crashes > 500
set $crashes=0
run
else
jump *Host_AbortCurrentFrame
end
end
run
```





# buildru2017
# CMake (рекомендуется)

Если вы хотите включить поддержку VGUI, вам понадобится часть Half-Life 1 SDK для сборки библиотеки vgui_support.

**ПРИМЕЧАНИЕ**: VGUI существует только для Linux/MacOSX/Windows и только для x86. Отключите его с помощью -DXASH_VGUI=no, если вы ориентируетесь на неподдерживаемую архитектуру.

**Клонировать репозиторий Xash3D с помощью git:**

``git clone --recursive https://github.com/FWGS/xash3d```

**Переместить в папку Xash3D:**

```cd xash3d```

**Клон Half Life 1 SDK с помощью git:**

```клон git https://github.com/ValveSoftware/halflife hlsdk/```

**ИЛИ** вы можете использовать репозиторий vgui-dev с только что скопированными заголовками VGUI и библиотекой. Клонируйте его с помощью git:

```клон git https://github.com/FWGS/vgui-dev hlsdk/```

**Подразумевая, что заголовки и библиотека VGUI клонированы в hlsdk, вы сможете собрать Xash3D как:**

```
mkdir -p построить
сборка компакт-диска
```

**В 32-битных системах:**

```cmake -DHL_SDK_DIR=../hlsdk -DXASH_SDL=yes -DXASH_VGUI=yes ../ && make```

**В 64-битных системах:**

``cmake -DHL_SDK_DIR=../hlsdk -DXASH_SDL=yes -DXASH_VGUI=yes -DCMAKE_C_FLAGS="-m32" -DCMAKE_CXX_FLAGS="-m32" -DCMAKE_EXE_LINKER_FLAGS="-m32" ../ && make```

**ПРИМЕЧАНИЕ. В Windows вам необходимо передать дополнительные ключи для целевой конкретной версии Visual Studio и установить путь SDL2. Например:**

```-G "Visual Studio 15 2017" -DSDL2_PATH="c:\projects\sdl2\"```

** для сборки с помощью Visual Studio 2017. Дополнительную информацию см. в документации по CMake. Вы можете скачать файлы разработки SDL2 на официальном сайте.**

**ПРИМЕЧАНИЕ. Сам Xash3D FWGS поддерживает 64-битную сборку, но из-за ее бесполезности она не разрешена по умолчанию. Если вы действительно знаете, ПОЧЕМУ вам нужна 64-битная сборка Xash3D FWGS, передайте -DXASH_64BIT=yes в cmake.**

См. другие возможные конфигурации сборки в нашей вики: Возможные конфигурации
Ручная сборка (без CMake)
Опираясь на Windows
Современная визуальная студия

Решение для VS2017 находится в msvc/dir в корневом каталоге репозитория. По крайней мере должно быть.
Визуальная студия 6

Это устаревшая конфигурация, но MSVC6, похоже, генерирует более стабильный и эффективный код.

**Настройте среду msvc6, распакуйте SDL2 в папку xash3d и выполните:**

```
компакт-диск (xash3d)\двигатель
..\msvc6\build.bat
cd (xash3d)\game_launch
..\msvc6\build_launch.bat
```

Создание игровых модов
линукс
микрондк

**Все моды, перенесенные на Android, могут быть собраны для Linux с помощью Android.mk с microndk:**

```[https://github.com/SDLash3D/microndk]```

Клонируйте репозиторий microndk где-нибудь, измените xash3d_config на предпочтительную конфигурацию (например, измените арку на x86)

**Перейдите в папку dlls, если вы строите сервер, или cl_dlls, если вы строите клиент, и выполните:**

```make -f /path/to/microndk/microndk.mk -j4```

**Делать:**

```сделать -f /path/to/microndk/microndk.mk -j4 чистым```

каждый раз, когда вы строите клиент после сборки сервера
Makefile.linux
Окна

В Windows обычным способом является использование Visual Studio, так как многие моды неправильно работают с mingw.

Просто откройте проект и создайте его.

Другой использует mingw и microndk, но еще не тестировался.

Моды на основе hlsdk-xash3d могут нормально работать с mingw.

Вы можете использовать microndk для его сборки. Процесс сборки очень похож на линуксовый.

**После настройки среды MinGW выполните:**
```mingw32-make -f \path\to\microndk\Microndk.mk```

**для сборки 64-битной библиотеки используйте:**
```mingw32-make -f \path\to\microndk\Microndk.mk 64BIT=1```

Отредактируйте xash3d_config.mk, чтобы установить оптимальные CFLAGS, если вы используете сервер.
Бег

**Скопировать папку Valve из Half-Life:**

```cp -r $HOME/.steam/steam/steamapps/common/Half-Life/valve $HOME/Games/Xash3D```

**ПРИМЕЧАНИЕ. Если вы выполняете сборку с помощью CMake, теперь вы можете использовать make install. Он установит двоичные файлы там, где они должны быть расположены. Так что просто запустите xash3d из командной строки в каталоге, где находятся игровые данные. Для получения дополнительной информации см. CMakeLists.txt в корне репозитория и скрипт xash3d.sh.**

**После успешной сборки скопируйте следующие файлы в другой каталог, где вы хотите запустить Xash3D:**

``cp engine/libxash.so game_launch/xash3d mainui/libxashmenu.so $HOME/Games/Xash3D```

**Если вы собрали его с помощью XASH_VGUI, также скопируйте vgui.so:**

```cp ../hlsdk/linux/vgui.so vgui_support/libvgui_support.so $HOME/Games/Xash3D```

**Копировать скрипт:**

```cp ../xash3d.sh $HOME/Games/Xash3D```

**Бежать:**

```
cd $HOME/Игры/Xash3D
./xash3d.sh
```


# руководство2017

** Поместите двоичные файлы xash3d и vgui (необязательно) в каталог данных игры и запустите: **

```LD_LIBRARY_PATH=. ./xash -dev 5```

Ручной запуск в Windows

После завершения сборки скопируйте следующие файлы в каталог по вашему выбору:

# Создается в процессе компиляции:
build/game_launch/[Конфигурация сборки]/xash.exe
build/engine/[Конфигурация сборки]/xash_sdl.dll
build/mainui/[Конфигурация сборки]/menu.dll
build/vgui_support/[Конфигурация сборки]/vgui_support.dll

# Автоматически загружаемые зависимости:
сборка/SDL2/[версия SDL]/lib/x86/SDL2.dll
сборка/VGUI/lib/win32_vc6/vgui.dll

# Из вашего каталога игр Steam:
Период полураспада/клапан

**Работает под управлением GDB**

```LD_LIBRARY_PATH=. gdb --args ./xash -dev 5```

Использование загрузчика DLL

**Поместите vgui_support.dll из сборки Windows в папку с игровыми данными и запустите