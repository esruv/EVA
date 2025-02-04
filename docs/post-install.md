# Post-Installation Instructions

## OOBE Setup

Note: Do not connect to the internet until told to do so.

Once you have begun the OOBE process, follow the steps in the video.

- Note: Do not enter a password by simply pressing enter, the service list recommended will break user password functionality & you will not be able to login again.

- See [media/oobe-windows7-example.mp4](https://raw.githubusercontent.com/amitxv/EVA/main/media/oobe-windows7-example.mp4)
- See [media/oobe-windows8-example.mp4](https://raw.githubusercontent.com/amitxv/EVA/main/media/oobe-windows8-example.mp4)
- See [media/oobe-windows10-example.mp4](https://raw.githubusercontent.com/amitxv/EVA/main/media/oobe-windows10-example.mp4)

## Activating Windows

As previously mentioned, you should have already linked a key to your motherboard but if you have not, now would be a good time to do so. Open CMD as administrator & enter the command below.

```bat
slmgr /ipk <25 digit key>
slmgr /ato
```

## Visual Cleanup

- Disable features on the taskbar, unpin shortcuts & tiles from the taskbar & start menu.

    - See [media/visual-cleanup-windows7-example.mp4](https://raw.githubusercontent.com/amitxv/EVA/main/media/visual-cleanup-windows7-example.mp4)
    - See [media/visual-cleanup-windows8-example.mp4](https://raw.githubusercontent.com/amitxv/EVA/main/media/visual-cleanup-windows8-example.mp4)
    - See [media/visual-cleanup-windows10-example.mp4](https://raw.githubusercontent.com/amitxv/EVA/main/media/visual-cleanup-windows10-example.mp4)

## Removing Bloatware

Before we remove bloatware via bruteforce on linux, we may as well uninstall what Windows allows us to.

- Open CMD as administrator & enter the command below.

    ```bat
    C:\prerequisites\scripts\remove-edge-onedrive.bat
    ```

- Uninstall bloatware in ``Control Panel > Programs > Programs & Features``.

    - In the ``Turn Windows features on or off`` section, disable everything **except** for:

        - Note: Keep ``Windows Search`` enabled on Windows 7

        - See [media/windows7-features-example.png](../media/windows7-features-example.png)

        - See [media/windows8+-features-example.png](../media/windows8+-features-example.png)

	- Restart your PC once before following the next steps (important)

- Windows 10+ Only:

    - Uninstall bloatware in ``Settings > Apps > Apps & Features``

        - In the ``Optional features`` section, uninstall everything apart from ``Microsoft Paint``, ``Notepad`` & ``WordPad``

        - Restart your PC once before following the next steps (important)

- ## Removing Bloatware with Linux

    - Boot into Ventoy on your USB in BIOS & select the Linux Mint image. Select ``Start Linux Mint`` when promted

    - Open the file explorer which is pinned to the taskbar & in the left pane, navigate to the volume Windows is installed on. You can identify this by finding the drive that has the ``debloat.sh`` script in

    - Right click an empty space & select ``Open in Terminal``. This will open the bash terminal in the directory of the script for us so we do not need to CD to it manually

    - Type ``sudo bash debloat.sh`` to run the script & wait for it to finish. Ignore any ``No such file or directory`` errors

    - Windows 8+ Only:

        - Change the view to list view to make the contents more readable by selecting ``View > List View``

        - Delete the following folders:

            - ``/Program Files/WindowsApps``

            - ``/ProgramData/Packages``

            - ``/Users/<username>/AppData/Local/Microsoft/WindowsApps``

        - In ``/Users/<username>/AppData/Local/Packages`` delete everything except:

            - **Microsoft.Windows.ShellExperienceHost_cw5n1h2txyewy**
            - **windows.immersivecontrolpanel_cw5n1h2txyewy**

        - In ``/Windows/SystemApps`` delete everything except:

            - **ShellExperienceHost_cw5n1h2txyewy**

    - Once finished, empty the ``Trash`` in the file explorer & restart to boot back into Windows

- Once back into the Windows desktop, open CMD as administrator & enter the command below to remove leftover scheduled tasks.

    ```bat
    C:\prerequisites\scripts\scheduled-tasks\disable-tasks.exe
    ```

## Install [Visual C++ Redistributable Runtimes](https://github.com/abbodi1406/vcredist/releases)

```
C:\prerequisites\VisualCppRedist_AIO_x86_x64.exe
```

## Merge the Registry Files

- Open CMD as administrator & enter the command below to merge the registry files. Replace ``<winver>`` with the Windows version you are configuring (e.g 7, 8, 10 etc).

    ```bat
    C:\prerequisites\scripts\registry\apply-registry.exe --winver <winver>
    ```

- Please ensure that the program prints a "done" message to the console, if it has not then command prompt was probably not opened with administrator privileges & the registry files were not successfully merged.

- Restart your PC (important).

- You may establish an internet connection after you have restarted as the Windows update policies will take effect.

## Install [OpenShell](https://github.com/Open-Shell/Open-Shell-Menu) (Windows 8+)

This step is required as we removed the spyware stock start menu

- Run ``OpenShellSetup.exe`` in ``C:\prerequisites\open-shell``

    - Only install the ``Open-Shell Menu``. Disable everything else to prevent installing bloatware

- I have included a registry file that will apply a basic OpenShell skin along with a few other settings, feel free to use your own

- Create a shortcut in win + r, ``shell:startup`` pointing to ``C:\Program Files\Open-Shell\StartMenu.exe``

- Windows 8 Only:

    - Open ``"C:\Program Files\Open-Shell\Start Menu Settings.lnk"``, enable ``Show all settings`` then go to the Windows 8 Settings section & set ``Disable active corners`` to All

## Miscellaneous

- Allow users full control of the ``C:\`` drive. This resolves an issue with xperf etl processing on Windows 7.

    - See [media/full-control-example.png](../media/full-control-example.png)

    - Click continue & ignore errors

- Open CMD & enter the command below.

    ```bat
    C:\prerequisites\scripts\miscellaneous.bat
    ```

- Enable ``Launching applications & unsafe files`` in ``Internet Options > Security > Custom Level``. This prevents [this annoying warning](https://gearupwindows.com/how-to-disable-open-file-security-warning-in-windows-10/). Feel free to skip this step as security may be reduced.

- In ``Advanced System Settings``, do the following:

    - In ``Computer Name > Change`` configure the PC name

    - In ``System Protection``, disable & delete system restore points. It has been proven to be very unreliable

- In ``Defragment & Optimize Drives``, disable **Run on a schedule**. More details on doing maintenance tasks ourself in [Final Thoughts & Tips](#final-thoughts--tips).

- Disable all messages in ``Control Panel> System & Security > Action Center > Change Action Center settings > Change Security & Maintenance settings``.

    - Note: This section is named ``Security & Maintenance`` on Windows 10+

- Restart your PC once before following the next steps (important)

## Installing Drivers

- Install any drivers your system requires, avoid installing chipset drivers.

- Try to obtain the bare driver so it can be installed in device manager as executable installers usually come with extra unnecessary bloatware. Most of the time, you can extract the installer's executable to obtain the driver.

- I would recommend updating & installing ethernet, USB, sata (required on Windows 7 as enabling MSI on the stock sata driver will result in a BSOD), NVME & potentially the audio controller drivers.

## Installing Recommended Packages & Programs

- Install [7-Zip](https://www.7-zip.org).

    - Run ``C:\prerequisites\7-Zip\7z2200-x64.exe``

        - Open ``C:\Program Files\7-Zip\7zFM.exe``, to go ``Tools > Options`` & associate 7-Zip with all file extensions by clicking the + button. You may need to click it twice to override existing associated extensions

- Web Browser.

    - See https://privacytests.org/

    - [Librewolf](https://librewolf.net) (fork of Firefox) recommended

    - The [Arkenfox user.js](https://github.com/arkenfox/user.js) is an alternative for Librewolf & applicable to a default Firefox installation

        - Open CMD & enter the command below

            ```bat
            C:\prerequisites\scripts\librewolf-web-installer.bat
            ```

        - Remove the following from ``C:\Program Files\LibreWolf``

            - ``pingsender.exe``

        - If you would like to set the search engine to Google, open [this link](https://www.linuxmint.com/searchengines.php), scroll to the bottom, click the Google icon & right click the URL to add the search engine to settings

        - Recommended ``about:config`` changes (enter about:config in the URL box). Thanks to Dato for initially sharing these

            - **Enable Compact Mode**

                - browser.uidensity = 1

            - **Remove fullscreen transition animation & warning message**

                - full-screen-api.transition-duration.enter = 0

                - full-screen-api.transition-duration.leave = 0

                - full-screen-api.warning.timeout = 0

            - **Remove tab preview image when dragging**

                - nglayout.enable_drag_images = false

            - **Disable reader mode**

                - reader.parse-on-load.enabled = false

            - **Disable ResistFingerprinting** (not recommended but the browser can become sluggish)

                - privacy.resistFingerprinting = false

        - Alternatively, the following lines can be added to ``"%userprofile%\AppData\Roaming\librewolf\Profiles\<profile>\prefs.js"``, ensure librewolf is closed before editing prefs.js

            ```
            user_pref("browser.uidensity", 1);
            user_pref("full-screen-api.transition-duration.enter", "0");
            user_pref("full-screen-api.transition-duration.leave", "0");
            user_pref("full-screen-api.warning.timeout", "0");
            user_pref("nglayout.enable_drag_images", false);
            user_pref("reader.parse-on-load.enabled", false);
            user_pref("privacy.resistFingerprinting", false);
            ```

    - Install [uBlock Origin](https://github.com/gorhill/uBlock), Librewolf already ships with it

        - I would also recommend importing [ClearURL's filter list](https://raw.githubusercontent.com/DandelionSprout/adfilt/master/ClearURLs%20for%20uBo/clear_urls_uboified.txt) along with [Dreammjow's filter list](https://raw.githubusercontent.com/dreammjow/MyFilters/main/src/filters.txt) & installing the [Skip Redirect](https://addons.mozilla.org/firefox/addon/skip-redirect/) extension

- Install [.NET 4.8 Runtimes](https://dotnet.microsoft.com/en-us/download/dotnet-framework/net48).

    - Run ``C:\prerequisites\ndp48-web.exe``

- Install [DirectX Runtimes](https://www.microsoft.com/en-gb/download/details.aspx?id=35).

    - Run ``C:\prerequisites\dxwebsetup.exe``, ensure to uncheck the bing bar option

- Media Player.

    - [mpv](https://mpv.io) / [mpv.net](https://github.com/stax76/mpv.net) or [mpc-hc](https://mpc-hc.org) ([alternative link](https://github.com/clsid2/mpc-hc)) recommended

## Replace Task Manager with Process Explorer

This step is not optional, pcw.sys will be disabled which breaks the stock Task Manager functionality.

<details>
<summary>Reasons not to use Task Manager</summary>

- It relies on a kernel mode driver (pcw.sys) to operate (additional overhead).

- Does not provide performance metrics such as cycles/ context switches delta & other useful details.

- On Windows 8+, [Task Manager reports CPU utility in %](https://aaron-margosis.medium.com/task-managers-cpu-numbers-are-all-but-meaningless-2d165b421e43) which provides misleading CPU utilization details, on the other hand, Windows 7's Task Manager & process explorer report time-based busy utilization. This also explains why the disable idle power plan option results in 100% CPU utilization on Windows 8+.
</details>

- Place ``C:\prerequisites\sysinternals\procexp.exe`` into ``C:\Windows`` & open it.

- Go to ``Options`` & select ``Replace Task Manager``. I also configure ``Confirm Kill`` & ``Allow Only One Instance``.

## Configure the BCD Store

- Open CMD & enter the commands below.

    - Disable the boot manager timeout when dual booting does not affect single boot times

        ```bat
        bcdedit /timeout 0
        ```
    - Configure [Data Execution Prevention](https://docs.microsoft.com/en-us/windows/win32/memory/data-execution-prevention) for ``essential Windows programs & services only``

        ```bat
        bcdedit /set nx optin
        ```

    - Configure the operating system name, I usually name it to whatever Windows version I am using e.g ``Windows 10 1803``

        ```bat
        bcdedit /set {current} description "OSNAME"
        ```

    - Windows 8+ Only
        
        - Implemented as a power saving feature for laptops & tablets, you absolutely do not want a [tickless kernel](https://en.wikipedia.org/wiki/Tickless_kernel) on a desktop

            ```bat
            bcdedit /set disabledynamictick yes
            ```

        - Forces the clock to be backed by a platform source, no synthetic timers are allowed. Have not been able to prove the benifits of this, feel free to skip or test yourself

            ```bat
            bcdedit /set useplatformtick yes
            ```

        - Configure the TSC synchronization policy. Have not been able to prove the benifits of this, feel free to skip or test yourself

            ```bat
            bcdedit /set tscsyncpolicy [legacy | enhanced]
            ```

            - Related: [research.md - What TscSyncPolicy does Windows use by default?](research.md#what-tscsyncpolicy-does-windows-use-by-default)

## Configure Memory Management Settings (Windows 8+)

- Open powershell & enter the command below.

    ```powershell
    Get-MMAgent
    ```

- If anything is set to True, use the command below as an example to disable a given setting.

    ```powershell
    Disable-MMAgent -MemoryCompression
    ```

## Disable Process Mitigations (Windows 10 1709+)

- Run the ``C:\prerequisites\scripts\disable-process-mitigations.bat`` script to disable [process mitigations](https://docs.microsoft.com/en-us/powershell/module/processmitigations/set-processmitigation?view=windowsserver2019-ps).

    - Effects can be viewed with the command below in powershell

        ```powershell
        Get-ProcessMitigation -System
        ```

## Configure the Network Adapter

- Open ``Network & Sharing Center > Change adapter settings``.

- Right click your main network adapter & select properties.

- Disable all items except ``QoS Packet Scheduler`` & ``Internet Protocol Version 4 (TCP/IPv4)``.

- [Configure a Static IP address](https://youtu.be/5iRp1Nug0PU?t=36), ths is required as we will be disabling the network services that waste cpu time.

- Disable ``NetBIOS over TCP/IP`` in ``General > Advanced > WINS`` to [prevent unnecessary system listening](https://github.com/djdallmann/GamingPCSetup/blob/master/CONTENT/DOCS/NETWORK/README.md).

## Preferences

- Go through the ``C:\prerequisites\preference`` folder to configure the following:

    - Configure Pointer Scheme

    - Desktop Icon Settings

    - Region & language

    - Taskbar Settings

    - Visual Effects & Pagefile

        - I usually just hit ``Adjust for best performance``

        - Ensure Desktop Composition is disabled on Windows 7

## Configure the Graphics Driver

- See [docs/configure-nvidia.md](../docs/configure-nvidia.md).

- See [docs/configure-amd.md](../docs/configure-amd.md).

## Configure MSI Afterburner

If you usually use [MSI Afterburner](https://www.msi.com/Landing/afterburner/graphics-cards) to configure the clock speed, fan speed & other settings, download & install it.

- Disable update checks & the low-level IO driver in settings.

- I would recommend configuring a static fan speed as using the fan curve feature requires the program to run continually.

- To automatically load a profile at startup, create a batch script in win + r, ``shell:startup`` containing the following, edit to suit your needs:

    ```bat
    @echo off
    setlocal EnableDelayedExpansion

    set "afterburner_path=C:\Program Files (x86)\MSI Afterburner\MSIAfterburner.exe"
    set "profile=1"

    if not exist "!afterburner_path!" (
        echo error: afterburner path invalid
        pause
        exit /b 1
    )

    start "" "!afterburner_path!" -Profile!profile!
    timeout -t 8 /nobreak
    powershell -command stop-process -name "MSIAfterburner" -force
    exit /b 0
    ```

## Configure CRU

If you usually use [Custom Resolution Utility](https://www.monitortests.com/forum/Thread-Custom-Resolution-Utility-CRU) to configure display resolutions, download & extract it.

- See [How to setup Display Scaling, works with all games | KajzerD](https://www.youtube.com/watch?v=50itBs-sz1w).

- Use the exact timing for an integer refresh rate.

- Try to delete every resolution & the other bloatware (audio blocks) apart from your native resolution, this may be a work around for the 1 second black screen when alt-tabbing in FSE, feel free to skip this step if you do not want to risk a black screen or are not comfortable with doing this.

- Restart your PC instead of using ``restart64.exe`` as it may result in a black screen.

- Ensure your resolution is configured properly in Display Adapter Settings.

    - Use the ``C:\prerequisites\change-resolution.lnk`` shortcut on Windows 8+

## Configure Services & Drivers

The service list configuration is not intended for laptop, Wi-Fi & webcam functionality. I am not responsible if anything goes wrong or you BSOD. The idea is to disable services while gaming & use default services for everything else.

- Download [Service-List-Builder](https://github.com/amitxv/Service-List-Builder/releases)

- On Windows 7 & 8, remove ``MMCSS`` from the ``DependOnService`` registry key in ``HKLM\SYSTEM\CurrentControlSet\Services\Audiosrv``.

- On 1607 & 1703, delete the ``ErrorControl`` registry key in ``HKLM\SYSTEM\CurrentControlSet\Services\Schedule`` to prevent an unresponsive explorer shell.

- Once configured, use the following command. The scripts will be built in the ``build`` folder & NSudo is required to run the batch scripts.

    ```bat
    service-list-builder.exe --config C:\prerequisites\bare-services.ini
    ```

- Move the scripts somewhere safe such as in the C:\ drive & do not share it with other people as it is specific to your system.

- Run ``Services-Disable.bat`` with NSudo, ensure ``Enable All Privileges`` is enabled (important).

## Configure Audio Devices

- Open the sound control panel, can be opened with win + r, ``mmsys.cpl``.

- Disable unused Playback & Recording devices.
    
- Disable audio enhancements as they waste cpu time.

    - See [media/audio enhancements-benchmark.png](../media/audio%20enhancements-benchmark.png)
    
- Disable Exclusive Mode in the Advanced section.

- I also like to set the sound scheme to no sounds in the Sounds tab.

## Configure Device Manager

Many devices in device manager will appear with a yellow icon as we ran the disable services script, **DO NOT** disable any device with a yellow icon however tempting it may be as as this will completely defeat the purpose of building toggle scripts. I would **highly** advise against asking other people for help with this step as they are almost guaranteed to tell you to "disable devices with a yellow icon" but as previously mentioned & i can not exaggerate this enough, this will completely defeat the purpose of building toggle scripts. I have reasons & specific methods for everything within this guide.

- Open device manager, ``View > Devices by connection``.

    - Disable write-cache buffer flushing on all drives in the ``Properties > Policies`` section.

    - Go to your ``network adapter > properties > advanced``, disable any power saving & wake features.

        - Related: [research.md - How many Rss Queues do you need?](research.md#how-many-rss-queues-do-you-need)

    - Disable the ``High Definition Audio Controller`` on the same PCI port as your GPU.

- Go to ``View > Resources by connection``.

    - Disable any **unneeded** devices that are using an IRQ or I/O resources, always ask if unsure, take your time on this step. Windows should not allow you to disable any required devices but ensure you do not accidentally disable another important device such as your main USB controller or similar. Once again, **DO NOT** disable any device with a yellow icon

        - If there are multiple of the same devices & you are unsure which one is in use, refer back to the tree structure in ``View > Devices by connection``. Note that a single device can use many resources. You can also use ``C:\prerequisites\MSIUtil.exe`` to check for duplicate, unneeded devices incase you accidently miss any with the confusing device manager tree structure

- Run ``Services-Enable.bat`` with NSudo, ensure ``Enable All Privileges`` is enabled (important).

- Open CMD & enter the command below to disable power saving for various devices in device manager.

    ```bat
    C:\prerequisites\scripts\disable-pnp-powersaving.ps1
    ```

- Open CMD & enter the command below to cleanup hidden & unused devices.
    
    ```bat
    C:\prerequisites\device-cleanup\DeviceCleanup.exe -s -n *
    ```

## Configure Control Panel

- It is not a bad idea to skim through both the legacy control panel & immersive control panel to ensure nothing is misconfigured, only takes a few minutes to do this anyway.

## Configure Power Options

- Set the power plan to high performance in ``Control Panel > Hardware & Sound > Power Options`` (PBO users excluded).

- Open CMD & enter the command below to remove every powerplan except the active power scheme, ignore errors.

    ```bat
	powercfg -delete 381b4222-f694-41f0-9685-ff5bb260df2e
	powercfg -delete 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c
	powercfg -delete a1841308-3541-4fab-bc81-f71556f20b4a
	powercfg -delete e9a42b02-d5df-448d-aa00-03f14749eb61
	```

- Open ``C:\prerequisites\PowerSettingsExplorer.exe`` & configure the following.

    - Allow Throttle States - Off

    - USB 3 Link Powermanagement - Off

    - USB Selective Suspend - Disabled

## Message Signaled Interrupts

Message signaled interrupts (MSIs) are faster than traditional line-based interrupts & may also resolve the issue of shared interrupts which are often the cause of high interrupt latency & stability
issues [[1](https://repo.zenk-security.com/Linux%20et%20systemes%20d.exploitations/Windows%20Internals%20Part%201_6th%20Edition.pdf)].

- Open ``C:\prerequisites\MSIUtil.exe``

    - Enable Message Signaled Interrupts (MSI) on devices that support it

        - You will BSOD if you enable MSI for the **stock** Windows 7 sata driver which you should have updated as mentioned in the [Installing Drivers](#installing-drivers) section
        
    - Be careful as to what you choose to prioritize as more harm than good may be done. E.g you will likely stutter in a open-world game that utilizes texture streaming if the GPU IRQ priority is set higher than the storage controller priority

- Restart your PC, you can verify if a device is utilizing MSIs by checking if it has a negative IRQ in MSIUtil

- Ensure that there is no IRQ sharing on your system by checking win + r, ``msinfo32`` ``Hardware Resources > Conflicts/Sharing`` section

## Interrupt Affinity

By default, CPU 0 handles the majority of DPCs & interrupts for several devices which can be viewed in a xperf dpcisr trace. This is not desirable as there will be a latency penalty because many processes & system activities are scheduled on the same core. We can use ``C:\prerequisites\Interrupt-Affinity-Tool.exe`` to set an interrupt affinity policy to the USB, GPU & NIC driver, which are few of many devices responsible for the most DPCs/ISRs, to offload them onto another core. They all require testing as you may do more harm than good if it is set to a weaker or equally as busy core.

- The correct device can be identified by cross-checking the ``Location Info`` with the ``Location`` in the ``properties > general`` section of a device in device manager

- Ideally you should use [AutoGpuAffinity](https://github.com/amitxv/AutoGpuAffinity) to benchmark the GPU affinity

- Use [Mouse Tester](https://github.com/microe1/MouseTester) to compare polling stability between the USB controller on different cores

    - Ideally this should be done with some sort of realistic load such as a game running in the background as idle benchmarks may be misleading, but as we do not have any games installed yet, you can come back & test this later

- Note: Restart your PC instead of an individual driver to avoid issues

- Open CMD & enter the command below to configure what CPU handles DPCs/ISRs for the network driver. Ensure to change the driver key to suit your needs.

    - Run ``C:\prerequisites\scripts\get-driver-keys.bat`` to get the driver keys on your system

        ```bat
        reg.exe add "HKLM\SYSTEM\CurrentControlSet\Control\Class\{4d36e972-e325-11ce-bfc1-08002be10318}\0000" /v "*RssBaseProcNumber" /t REG_SZ /d "2" /f
        ```

- You can ensure interrupt affinity policies have been configured correctly by analyzing a xperf trace while the device is busy

## Memory Cleaner (Windows 10 1909 & Under)

Feel free to skip this step as it is not required, Microsoft fixed the standby list memory management issues in a later version of Windows. [Memory Cleaner](https://github.com/danskee/MemoryCleaner) ([alternative link](https://git.zusier.xyz/Zusier/MemoryCleaner)) also allows us to set the kernel timer-resolution globally however the behaviour of timer-resolution changed in 2004+ as explained in [this article](https://randomascii.wordpress.com/2020/10/04/windows-timer-resolution-the-great-rule-change/), rendering this trick useless.

- Place ``C:\prerequisites\Memory-Cleaner.exe`` in win + r, ``shell:startup`` & open it.

    - Go to ``File > Settings`` & configure:
    
        - The hotkey to clean the standby list & working set

        - The desired timer-resolution, 10000 (1ms) recommended

        - Uncheck ``Enable timer``

        - Check ``Start minimized`` & ``Start timer resolution automatically``

- Avoid using auto cleaning apps like ISLC/Memreduct, they consume alot of resources due to a frequent polling timer interval & cause stuttering due to autoclearing memory.

## Optimizing the File System

- Open CMD & enter the commands below.

    - Disables the creation of 8.3 character-length file names on FAT- & NTFS-formatted volumes

        ```bat
        fsutil behavior set disable8dot3 1

    - Disable the encryption of folders & files on NTFS volumes

        ```bat
        fsutil behavior set disableencryption 1
        ```

    - Disable updates to the Last Access Time stamp on each directory when directories are listed on an NTFS volume

        ```bat
        fsutil behavior set disablelastaccess 1
        ```

    - Configure NTFS quota violations to be reported in the system log every 3 hours instead of every hour

        ```bat
        fsutil behavior set quotanotify 10800
        ```

    - Enables delete notifications (also known as trim or unmap), should be enabled by default but here for safe measure

        ```bat
        fsutil behavior set disabledeletenotify 0
        ```

## Disable Hidden Power Saving

All hidden means is not visible to the user, many driver INF configuration files contain these registry entries that are clearly labeled power saving, however I have not been able to prove the benifit of this script so feel free to skip this step.

- Run the ``C:\prerequisites\scripts\disable-hidden-powersaving.bat`` script

## Installing Games & Applications

Now is a good time to install whatever programs you commonly use to prepare us for the next steps.

## Configure FSE & QoS for Games

- Microsoft has claimed FSO/independent flip has improved in later Windows versions which has also been verified by members in the community with [Reflex Latency Analyzer](https://www.nvidia.com/en-gb/geforce/news/reflex-latency-analyzer-360hz-g-sync-monitors), however other users have claimed otherwise, my suggestion would be to test both & use whatever feels acceptable.

- Configuring a QoS Policy will allow Windows to prioritize packets of an application over other devices on your network & PC.

    - Related: [research.md - How can you verify if a DSCP QoS policy is working?](research.md#how-can-you-verify-if-a-dscp-policy-is-working)

- Run the ``C:\prerequisites\scripts\fse-qos-for-game-exes.bat`` script & follow the instructions in the console output

## Cleanup

- Clear the PATH user environment variable of locations pointing to Windows bloatware folders

- Some locations you may want to review for leftover bloat & unwanted shortcuts

    - ``"%userprofile%\AppData"``
    - ``"%userprofile%\AppData\Local\Temp"``
    - ``"%userprofile%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs"``
    - ``"%userprofile%\Downloads"``
    - ``"C:\"``
    - ``"C:\ProgramData\Microsoft\Windows\Start Menu\Programs"``
    - ``"C:\Program Files"``
    - ``"C:\ProgramData"``
    - ``"C:\Windows\Prefetch"``
    - ``"C:\Windows\SoftwareDistribution\download"``
    - ``"C:\Windows\Temp"``

- Reset Firewall rules:

    - Open CMD & enter the command below

        ```bat
        reg.exe delete "HKLM\System\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules" /f
        reg.exe add "HKLM\System\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules" /f
        ```

- Open ```C:\prerequisites\sysinternals\Autoruns.exe``` & remove any unwanted programs such as game launchers from starting automatically. Remove all obsolete entries with a yellow label, run with NSudo if you encounter any permission errors.

- Configure Disk Cleanup:

    - Open CMD & enter the command below, tick all of the boxes

        ```bat
        cleanmgr /sageset:50
        ```
    - Run Disk Cleanup:

        ```bat
        cleanmgr /sagerun:50
        ```

## Configure Default Programs

- Configure default programs in ``Settings > Apps``.

## Final Thoughts & Tips

- While gaming, consider the following:

    - Killing explorer.exe after you launch your game, it uses a ton of cycles.

        - Use ``ctrl + shift + esc`` to open process explorer then use ``File > Run`` to start the ``explorer.exe`` shell again

    - Disabling idle states (PBO users excluded) which will force C-State 0 & eliminate jitter due to the process of state transition. After all, C1 is still power saving [[1](https://www.dell.com/support/kbdoc/en-uk/000060621/what-is-the-c-state)].

        - Drag & drop the scripts in ``C:\prerequisites\scripts\idle-scripts`` to the desktop for easy access. This way you can disable idle before launching a game & re-enable it after you close your game

    - Kill other processes that waste cpu time such as game clients

- Don't run random tweaks, tweaking programs or fall for the "fps boost" marketing nonsense. If you have a question about a specific option or setting, just ask.

- Try to favour FOSS (free & open source software). Stay away from proprietary software where you can.

- Ensure to scan files with [VirusTotal](https://www.virustotal.com/gui/home/upload) before running them.

- Cap your framerate at a multiple of your monitor refresh rate to prevent frame mistiming [[1](https://youtu.be/_73gFgNrYVQ)]. E.g possible framerate caps with a 144hz monitor include 72, 144, 288, 432 ...

- Capping your framerate with [RTSS](https://www.guru3d.com/files-details/rtss-rivatuner-statistics-server-download.html) instead of the ingame limiter will result in consistent frametimes & a smoother experience but at the cost of noticeably higher latency.

- Carry out maintenance tasks yourself on a weekly basis. This includes:

    - Trimming your SSD

    - Using a [lint roller](https://www.ikea.com/gb/en/p/baestis-lint-roller-grey-90425626) to remove dirt & debris from the mousepad once in a while

    - Using a small [air dust blower](https://www.amazon.co.uk/s?k=air+dust+blower) to remove dirt & debris from the mouse sensor lens often

    - Removing dust from components often
