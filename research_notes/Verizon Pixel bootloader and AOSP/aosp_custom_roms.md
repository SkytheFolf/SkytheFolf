# AOSP and Custom ROMs on Google Pixel Phones (as of October 2026)

Research note on method: the egress proxy blocked direct fetches of grapheneos.org, calyxos.org, source.android.com, androidauthority.com and 9to5google.com. Findings come from search-result extracts of those pages plus secondary coverage. developer.android.com/dsu was fetched directly. Claims sourced only from forums or secondary blogs are flagged as such.

## 1. What AOSP is, and how Google changed AOSP publishing in 2025-2026 (impact on Pixel ROMs)

### Takeaway
AOSP is still open source, but Google now develops Android privately (since March 2025). It has published source only in two drops a year (since 2026), and since Android 16 (June 2025) it has stopped shipping Pixel device trees and driver binaries. Pixel ROMs therefore have more work to do and lag stock Pixel releases by months, which matters most for the QPR releases.

### Cited Findings
- **Private development (March 2025):** Android Authority reported on March 26, 2025 that Google would move all Android OS development to its internal branch. Work that used to be visible on public AOSP Gerrit now happens privately, and source is published only after each release. Google's stated reason is to "streamline its development process": the public branch lagged the internal one, which caused merge conflicts. — [Android Authority](https://www.androidauthority.com/google-streamlining-android-development-3538503/); [Android Authority follow-up](https://androidauthority.com/aosp-development-private-help-3539648); [Gigazine](https://gigazine.net/gsc_news/en/20250327-google-android-os-development-in-private); [Notebookcheck](https://www.notebookcheck.net/Google-insists-AOSP-will-remain-open-source-despite-switch-to-private-development.989251.0.html)
- **Branches:** the public `aosp-main` branch became read-only. Google tells platform developers to use the `android-latest-release` manifest branch, which always points to the newest release pushed to AOSP. External patches can still go to public AOSP Gerrit, and GMS-licensed partners use a partner Gerrit. — [9to5Google](https://9to5google.com/?p=666914); [TechRadar](https://www.techradar.com/pro/google-set-to-reduce-the-android-source-code-releases-to-just-twice-a-year)
- **Twice-yearly source drops (from 2026):** Android Authority reported in January 2026 that, starting in 2026, Google publishes new source to AOSP only in Q2 (the major release) and Q4 (the minor release). Before this, every quarterly release was published. Google gave platform stability and alignment with its "trunk-stable" development model as the reasons. Android itself still gets four releases a year, and security patches still ship through a dedicated security-only branch. — [Android Authority](https://androidauthority.com/aosp-source-code-schedule-3630018/); [TechRadar](https://www.techradar.com/pro/google-set-to-reduce-the-android-source-code-releases-to-just-twice-a-year); [Slashdot](https://tech.slashdot.org/story/26/01/06/2154225/); [OpenSourceForU](https://www.opensourceforu.com/?p=95155)
- **Android 16 dropped Pixel device support from AOSP (June 2025):**
  - The Android 16 AOSP release did not include Pixel device trees or driver binaries.
  - Google is moving AOSP's reference target from Pixel hardware to the virtual device "Cuttlefish".
  - The commit history of the Pixel kernel source was squashed.
  - Android VP Seang Chau said "AOSP is NOT going away", but Google's statement suggested Pixel device trees will not come back.
  - Sources: [9to5Google](https://9to5google.com/2025/06/12/android-open-source-project-pixel-change/); [Android Authority](https://androidauthority.com/google-not-killing-aosp-3566882); [droid.tools](https://droid.tools/google-aosp-not-discontinued)
- **Reactions from ROM projects:**
  - LineageOS's Nolen Johnson said the work would become "painful", with developers having to "blindly guess and reverse engineer" changes from prebuilt binaries.
  - CalyxOS said AOSP 16 "cannot currently be built or run on any recent Pixel device easily just using official source". — [9to5Google](https://9to5google.com/2025/06/12/android-open-source-project-pixel-change/); [CalyxOS blog](https://calyxos.org/news/2025/06/11/android-16-plans/)
- **How GrapheneOS adapted:**
  - GrapheneOS said Google had contacted it in April 2025 about removing device support.
  - It had to "massively overhaul adevtool to fully replace the need for Android device trees".
  - It says it now does not use AOSP device trees at all, which allowed it to add Pixel 10 support without them. — [Mastodon mirror (feddit)](https://feddit.it/post/20935012/15333671); [GrapheneOS forum](https://discuss.grapheneos.org/d/25099-pixel-10-support-for-grapheneos-is-now-available/233); [Developer-Tech](https://www.developer-tech.com/news/android-open-future-questioned-google-withholds-pixel-code/) (forum and Mastodon content, reported secondhand)
- **QPR lag in practice:**
  - Android 16 QPR1 shipped on Pixels on September 3, 2025, but reached AOSP only on November 11, 2025. — [GrapheneOS forum](https://discuss.grapheneos.org/d/25099-pixel-10-support-for-grapheneos-is-now-available/233)
  - Android 17 was released on June 16, 2026, and GrapheneOS shipped its port almost immediately.
  - Android 17 QPR1 reached Pixels on September 15, 2026. GrapheneOS says it adds APIs and security patches that are not in AOSP, so its port cannot be published until the code reaches AOSP with QPR2 in December 2026. Meanwhile GrapheneOS is reverse-engineering the fixes, and it says Google is not honoring its GPL source requests. — [anoni.net](https://anoni.net/docs/en/blog/2026/07/2026-grapheneos-android-17/); [next.ink](https://next.ink/256965/grapheneos-deplore-lattitude-de-google-sur-les-correctifs-de-securite-dandroid/); [GrapheneOS forum](https://discuss.grapheneos.org/d/36469-grapheneos-has-been-ported-to-android-17-and-official-releases-are-coming-soon)
- **CalyxOS on the same problem:** its August 2026 notes say that because Google stopped publishing source for the QPR1 and QPR3 releases, CalyxOS cannot add updated proprietary components (bootloader, modem, vendor firmware) on time. — [CalyxOS Aug 2026 security update](https://calyxos.org/news/2026/08/07/august-security-update/)

### Inferences
- In 2026, a Pixel ROM user should expect a gap of up to about 3 months between Google's stock QPR feature and firmware updates and the matching ROM release. Security patches still arrive through the security-only branch, although GrapheneOS disputes how complete they are.
- Projects with their own Pixel vendor-extraction tooling (GrapheneOS's adevtool) are less hurt by the loss of device trees than projects that relied on Google's device trees.
- Building "pure AOSP" for a recent Pixel is no longer a supported path. Cuttlefish is now the reference target.

### Gaps
- I could not fetch Google's own source.android.com wording on the device-tree removal or the Q2/Q4 schedule. The facts above come from press coverage.
- Whether Google ever partially restored Pixel device trees after June 2025 is unconfirmed. No source said it did.

## 2. Android Verified Boot (AVB): locked vs unlocked, why unlocking is needed, relocking with custom keys

### Takeaway
A locked Pixel bootloader boots only images signed with Google's key, or with a user-enrolled custom key in `avb_custom_key`. Flashing anything else requires `fastboot flashing unlock`, which wipes the device. Pixels let you enroll a custom AVB key and relock. The phone then boots in the "yellow" state, with a warning screen, but keeps verified boot and rollback protection. GrapheneOS and CalyxOS support relocking this way. LineageOS's normal install leaves the bootloader unlocked.

### Cited Findings
- Unlocking requires a confirmation on the device that you want to "erase all user data and unlock the device". Flashing a factory image also needs a full reset. — [developer.android.com factory images](https://developer.android.com/about/versions/16/download); [Android Authority](https://androidauthority.com/android-sites-help-unbrick-google-pixel-how-3681441)
- Text quoted from AOSP boot-flow documentation in forum threads: "When the custom key is set and the device is in the LOCKED state it will boot images signed with both the built-in key as well as the custom key. All other security features (including rollback-protection) are in effect." The yellow warning appears when the image is signed with a custom key. — [/e/ community forum quoting AOSP docs](https://community.e.foundation/t/feedback-for-v2-1/58216/198); [iodé community](https://community.iode.tech/t/any-way-to-avoid-your-phone-has-loaded-a-different-operating-system/7315)
- **Relock procedure for ROMs that support it:**
  1. Erase `avb_custom_key`.
  2. Flash the ROM's public key, for example a `pkmd_pixel.bin`-style file.
  3. Run `fastboot flashing lock`.
  - Failure case: if the images do not verify against the key, relocking fails with "invalid android images, skip locking". These are user forum reports, not official documentation. — [iodé community (Pixel 7)](https://community.iode.tech/t/pixel-7-cant-relock-bootloader-after-iode-install/5604); [/e/ community (Pixel 7)](https://community.e.foundation/t/pixel-7-impossible-to-relock-bootloader/65412)
- A boot warning is unavoidable with any non-stock OS. Only a locked bootloader running the vendor's own OS shows no warning. — [r/LineageOS mirror (2021)](https://redlib.hackliberty.org/r/LineageOS/duplicates/n7yo7u)
- **Verizon and "OEM unlocking" (carrier unlock is not bootloader unlock):**
  - "OEM unlocking" must be enabled in Developer options before you can unlock the bootloader.
  - CalyxOS: if the toggle is greyed out, "you may have the Verizon version". It also notes that unlocking the bootloader is different from carrier-unlocking a phone. — [CalyxOS Verizon page](https://calyxos.org/install/verizon/)
  - Google Pixel Community answer: phones bought from or associated with Verizon have bootloaders that cannot be unlocked, even after carrier unlock. — [Google Pixel Community](https://support.google.com/pixelphone/thread/318109388/oem-unlock-disabled-pixel-3xl?hl=en)
  - A Verizon representative: "We do not unlock the bootloader on our devices." — [Verizon Community](https://community.verizon.com/t5/Google-Pixel/Pixel-OEM-Unlocking-Support/td-p/1153573/page/2)
  - Factory reset while online:
    - One Reddit user claims the toggle became available after a factory reset with an internet connection. This is anecdotal.
    - A GrapheneOS forum thread about a Verizon Pixel 6a reports the toggle stayed greyed out after a factory reset.
    - Sources: [Reddit mirror](https://cal1.lr.ggtyler.dev/r/Pixel4a/comments/1ifvcxp/oem_unlock_greyed_out); [GrapheneOS forum](https://discuss.grapheneos.org/d/1552-oem-unlocking-procedure-on-verizon-variant)

### Inferences
- A relocked GrapheneOS or CalyxOS Pixel keeps the full verified-boot chain, rollback protection and hardware attestation. An unlocked LineageOS install gives up verified boot integrity against physical or persistent attackers.
- For a Verizon-variant Pixel with a greyed-out OEM-unlocking toggle, none of the custom ROM paths in this note can be installed.

### Gaps
- I could not retrieve the official GrapheneOS or CalyxOS install pages that describe their relock step (domains blocked). Relock support is well known for both projects, but this note cites it only indirectly.
- No 2026 primary source confirms whether Verizon still locks newer Pixels (Pixel 9 and 10). The evidence is older community threads.

## 3. Main custom ROM options for Pixels in 2026

### Takeaway
- **GrapheneOS:** the most active Pixel-only, security-focused OS. It supports Pixel 6 and later, including the whole Pixel 10 line and the 10a, and it relocks.
- **CalyxOS:** paused in mid-to-late 2025, resumed releases in July 2026, and now lags on Android versions and Pixel firmware. It relocks.
- **LineageOS 23.x (Android 16):** supports Pixel 4 through the Pixel 9 line, including the 9a. I found no confirmed Pixel 10 support. It stays unlocked.
- Every ROM needs an unlocked bootloader to install.

### Cited Findings
- **GrapheneOS, Pixel 10:**
  - The Pixel 10, 10 Pro, 10 Pro XL and 10 Pro Fold moved from experimental to production with release 2026011000 on January 10, 2026. Experimental builds started on November 25, 2025. — [GrapheneOS forum](https://discuss.grapheneos.org/d/25099-pixel-10-support-for-grapheneos-is-now-available)
  - The Pixel 10a got experimental support in March 2026 and later appears on the official list. Secondary sources list its minimum support end as March 2033, and August 2032 for the Pixel 10. — [efani blog](https://www.efani.com/blog/grapheneos-supported-devices); [GrapheneOS releases](https://grapheneos.org/releases) (secondary-blog details; verify against grapheneos.org)
- **GrapheneOS, other hardware:**
  - GrapheneOS has said support for the Pixel 11 and later is "not determined yet", and that it will keep supporting existing Pixels until their end of life.
  - It is working with a major OEM on non-Pixel hardware; secondary reports name Motorola, for 2026 or 2027.
  - No retail non-Pixel device is officially supported. — [AndroidHeadlines](https://androidheadlines.com/2025/10/grapheneos-may-break-pixel-exclusivity-with-a-new-phone-in-2026.html); [WinFuture](https://winfuture.de/news,154242.html); [efani blog](https://www.efani.com/blog/grapheneos-supported-devices)
- **GrapheneOS, Android 17:** ported and shipped right after the June 16, 2026 release, for all supported devices. — [anoni.net](https://anoni.net/docs/en/blog/2026/07/2026-grapheneos-android-17/); [GrapheneOS forum](https://discuss.grapheneos.org/d/36469-grapheneos-has-been-ported-to-android-17-and-official-releases-are-coming-soon)
- **CalyxOS status:**
  - Development paused in mid-2025 after key staff left. The project rebuilt its signing infrastructure and resumed.
  - Version 7.2.2.0 shipped on July 1, 2026 with OTA updates restored, and an Android 17 port was described as underway.
  - Sources: [reclaimthenet](https://reclaimthenet.org/recommended-tool/calyxos); [Vucense](https://vucense.com/comparisons-alternatives/app-versus/grapheneos-vs-calyxos-2026-comparison/); [CalyxOS Aug 2026 update](https://calyxos.org/news/2026/08/07/august-security-update/); [CalyxOS Aug 2026 feature update](https://calyxos.org/news/2026/08/28/august-feature-update/)
  - Conflict: Borncity reports that a CalyxOS 8.0 based on Android 17 came out around October 2, 2026. This is unconfirmed against calyxos.org. — [Borncity](https://borncity.com/news/calyxos-8-0-android-17-kommt-ohne-integrierte-google-dienste/)
- **CalyxOS device support:**
  - The official device page lists estimated end dates of April 2032 (Pixel 9a), August 2031 (Pixel 9 series) and May 2031 (Pixel 8a).
  - The Pixel 4a (5G) and 5a now get only open-source fixes.
  - I found no confirmed Pixel 10 support. — [CalyxOS device support](https://calyxos.org/docs/guide/device-support/); [CalyxOS Aug 2026 update](https://calyxos.org/news/2026/08/07/august-security-update/)
- **LineageOS 23.x:**
  - LineageOS 23.0 is based on Android 16.
  - Supported Pixels: Pixel 4/4 XL, 4a, 4a 5G, 5, 5a, 6/6 Pro/6a, 7/7 Pro/7a, Fold, 8/8 Pro/8a, and 9/9 Pro/9 Pro XL/9 Pro Fold. The Pixel 9a was added in November 2025.
  - A late-2025 report said the team was "hopeful" about the Pixel 10. Not confirmed since.
  - A less established site describes LineageOS 23.2 as based on Android 16 QPR1.
  - Sources: [9to5Google](https://9to5google.com/2025/11/04/lineageos-now-officially-supports-the-pixel-9a/); [Android Authority](https://www.androidauthority.com/google-pixel-4-android-16-lineageos-23-3628045); [Phonandroid](https://www.phonandroid.com/?p=2674807); [thecustomrom.com](https://thecustomrom.com/news/lineageos-23-2-material-expressive)
- **Google support context:** Swappa says the Pixel 6 series reaches the end of Google's support window around October 2026. — [Swappa](https://swappa.com/blog/?p=30874)

### Inferences
- For a Pixel 10 or 10a owner in October 2026, GrapheneOS is the only one of the three major projects with confirmed production support.
- LineageOS is the broadest option for older Pixels (the 4 and 5 series) that the other two no longer fully support.
- Other options (/e/OS, iodéOS) appear in relock forum threads for Pixels. I did not research their 2026 Pixel coverage.

### Gaps
- I could not verify LineageOS Android 17 (LineageOS 24) status or Pixel 10 support.
- The CalyxOS 8.0 / Android 17 release is unverified, as is any CalyxOS Pixel 10 support.
- I could not retrieve GrapheneOS's official device and legacy list (site blocked).

## 4. Building AOSP and flashing a Pixel

### Takeaway
Google's official guidance:
- A 64-bit x86 Linux machine with at least 64 GB of RAM and at least 400 GB of free disk.
- `repo init` on `android-latest-release`, `repo sync`, `source build/envsetup.sh`, `lunch <target>`, then `m`.
- Flash with `fastboot flashall` / `fastboot flash` or the browser-based Android Flash Tool. Both need an unlocked bootloader.

Since Android 16, recent Pixels lack the AOSP device trees needed for a plain build, so most Pixel owners install a ROM project's prebuilt images instead.

### Cited Findings
- **Hardware (source.android.com):**
  - At least 400 GB of free disk: 250 GB to check out the code plus 150 GB to build.
  - At least 64 GB of RAM, on a 64-bit x86 system.
  - The page initializes `android-latest-release` with a partial-clone flag, syncs in parallel, sources `build/envsetup.sh`, then picks a target with `lunch`. — [source.android.com "Try Android development"](https://source.android.com/setup/start)
  - Older or third-party guides give lower minimums (16 GB RAM/swap; 32 GB+ recommended). — [Qt AOSP build guide](https://doc.qt.io/QtAndroidAutomotive/building-aosp-system-image.html); [cnb.cool monorepo](https://cnb.cool/grq/monorepo)
- **Branch:** use `android-latest-release`, not `aosp-main`. — [9to5Google](https://9to5google.com/?p=666914); [TechRadar](https://www.techradar.com/pro/google-set-to-reduce-the-android-source-code-releases-to-just-twice-a-year)
- **Android 16 build limits:** CalyxOS said AOSP 16 cannot easily be built or run on recent Pixels from official source alone. Google's reference target is now Cuttlefish, a virtual device. — [CalyxOS](https://calyxos.org/news/2025/06/11/android-16-plans/); [9to5Google](https://9to5google.com/2025/06/12/android-open-source-project-pixel-change/)
- **Fastboot flashing:** `fastboot flashall -w` flashes all partitions and wipes `/data`. — [source.android.com Flash with Fastboot](https://source.android.com/docs/setup/build/adb)
- **Android Flash Tool (flash.android.com):**
  - Works in any WebUSB browser (Chrome, Edge 79+) with no local tools installed.
  - Requires an unlocked device and USB debugging, and erases all data. — [developer.android.com factory images](https://developer.android.com/about/versions/17/download); [Android Authority](https://androidauthority.com/android-sites-help-unbrick-google-pixel-how-3681441)
  - Google's Pixel Repair site can restore software on a locked bootloader. — [Android Authority](https://androidauthority.com/android-sites-help-unbrick-google-pixel-how-3681441)

### Inferences
- In practice, "building a ROM" for a modern Pixel means following the build docs of a specific project: GrapheneOS with adevtool vendor extraction, or LineageOS with `breakfast`/`brunch` device configs. Google no longer provides vanilla Pixel lunch targets for recent devices.

### Gaps
- I could not confirm which Pixel `lunch` targets, if any, remain in `android-latest-release` after Android 16, or the exact current command text on source.android.com (site blocked to direct fetch).

## 5. Options without unlocking: DSU/GSI, root, Shizuku/ADB debloating

### Takeaway
- **DSU:** available on Pixel 3 and later. It boots a GSI temporarily without overwriting the system, but only trusted GSIs (signed by Google or the OEM) are accepted. Community tools and guides consistently say custom or third-party GSIs need an unlocked bootloader.
- **Root (Magisk etc.):** requires unlocking.
- **Locked phones:** the realistic options are ADB `pm uninstall --user 0` or `pm disable-user`, or on-device tools such as Shizuku plus Canta.

### Cited Findings
- **DSU (Google docs):** "DSU depends on the Android Dynamic Partition feature and requires the GSIs to be signed by Google or your OEMs as a trusted system image." Google enabled DSU on Pixel 3 and newer starting with Android 10 Beta 4. Android 11 and later include a "DSU Loader" in Developer options. — [developer.android.com/dsu](https://developer.android.com/dsu)
- **DSU's design intent:** XDA described it as a way to boot a GSI without touching the existing installation, and as not requiring an unlocked bootloader or a data wipe. — [XDA](https://www.xda-developers.com/android-q-dynamic-system-updates-project-treble/amp/); [XDA Android 11 DSU Loader](https://www.xda-developers.com/android-11-dsu-loader-gsi-locked-bootloader-developers-test-apps-stock-android/)
- **Contrary evidence: unlock needed in practice:**
  - DSU Sideloader lists an unlocked bootloader as a requirement, because locked devices boot only OEM-signed images.
  - Droidwin guides report "corrupt" errors or getting stuck in fastboot when using DSU on a locked bootloader.
  - AOSP lets OEMs include "Developer GSI keys" so that developer GSIs boot while LOCKED. — [DSU Sideloader (F-Droid)](https://f-droid.org/packages/vegabobo.dsusideloader/); [XDA DSU Sideloader](https://www.xda-developers.com/dsu-sideloader-boot-custom-android-gsi/); [Droidwin](https://droidwin.com/?p=28977); [source.android.com DSU (ru)](https://source.android.com/devices/tech/ota/dynamic-system-updates?hl=ru)
  - Enterprise caveat: on managed devices where policy disables Developer options, DSU cannot start. — [Bayton](https://sysapps.bayton.org/packages/com.android.dynsystem)
- **Shizuku plus Canta:**
  - Canta runs the equivalent of `pm uninstall --user 0` on the phone with no PC or root, using Shizuku. It labels packages with risk levels from the Universal Debloater Alliance list.
  - Shizuku needs Developer options and (wireless) debugging, and must be restarted after every reboot.
  - Restore a removed app with `cmd package install-existing <pkg>`. — [Kevin Boone](https://kevinboone.me/canta.html); [Privacy Guides forum](https://discuss.privacyguides.net/t/canta-with-shizuku-for-uninstalling-bloatware-without-pc-or-root/16467)

### Inferences
- On a locked Verizon Pixel, DSU can at most boot Google-signed GSIs, which are essentially stock AOSP builds for testing, not a privacy ROM. Even this is uncertain on current builds, and in any case it is not a way to run GrapheneOS or LineageOS.
- Debloating with ADB or Shizuku removes apps for user 0 only. The APKs stay on the read-only system partition, and a factory reset restores them.
- Locked-phone alternatives therefore reduce bloat and tracking but cannot replace the OS, remove Google Play Services entirely, or add the hardening of GrapheneOS.

### Gaps
- I found no authoritative, current (Android 16/17) test confirming whether a Google-signed GSI actually boots through DSU Loader on a locked retail Pixel. Sources conflict.
- I found no source on rooting a locked Pixel without unlocking. Root via Magisk or KernelSU is widely understood to require an unlocked bootloader to flash a patched boot or init_boot image, but I did not retrieve a citation, and no locked-bootloader root exploit for current Pixels was found.
