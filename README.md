TeamWin Recovery Project (use omni-7.1 branch for porting from TRT-Lxx)
Use force 32-bit TWRP, omni-7.1 --- it boots!

Device configuration for Huawei Y7 Prime 2018 (LDN-L21B)
=====================================================

Basic   | Spec Sheet
-------:|:-------------------------
CHIPSET | Qualcomm MSM8937 Snapdragon 430
GPU     | Adreno 505
Shipped Android Version | Android 8.0
Release Date | April 2018

Fix building:

         mkdir out/target/product/london/recovery     
         mkdir out/target/product/london/recovery/root
         touch out/target/product/london/recovery/root/file_contexts

if omni-8.0 add:

    export ALLOW_MISSING_DEPENDENCIES=true
    export LC_ALL=C
    mka adbd recoveryimage
-----
Experiments about adding FBE decrypting of Internal Storage.
android_system_core/init/builtins.cpp
646 

        static int do_install_keyring(const std::vector<std::string>& args) {
        if (e4crypt_install_keyring()) {
        //ERROR("failed to install keyring\n");
        PLOG(ERROR) << "failed to install keyring\n";
        return -1;
        }
        property_set("ro.crypto.state", "encrypted");
        property_set("ro.crypto.type", "file");
        return 0;
        }
1196    
        
        {"insmod",                  {1,     kMax, do_insmod}},	
        {"install_keyring",         {0,     0,    do_install_keyring}},
        {"installkey",              {1,     1,    do_installkey}},
        {"load_persist_props",      {0,     0,    do_load_persist_props}},
        {"load_system_props", {0, 0, do_load_system_props}},
-----
see this: https://github.com/TeamWin/android_device_oneplus_cheeseburger/commit/e684edca5f714a2af163bd33e42725c5828ce397
or this: https://github.com/chetgurevitch/android_device_huawei_angler/commit/982198a17236ec04c2042a1e7097d8a855b2761e
