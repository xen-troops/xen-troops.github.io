# Test KeyMaster and GateKeeper

We are using corresponding VTS tests for KeyMaster and GateKeeper.

## Build tests
Assuming that you have downloaded sources, properly set environment etc.
```
cd hardware/interfaces/keymaster/3.0/vts/functional
mm
cd hardware/interfaces/gatekeeper/1.0/vts/functional
mm
```

## Use prebuilt binaries
You can download [zip archive](gkkm_vts_prebuilt.zip) with already built binaries.

## Push binaries to board
```
adb push ${ANDROID_PRODUCT_OUT}/testcases/VtsHalKeymasterV3_0TargetTest/arm64/VtsHalKeymasterV3_0TargetTest /data/
adb push ${ANDROID_PRODUCT_OUT}/testcases/VtsHalGatekeeperV1_0TargetTest/arm64/VtsHalGatekeeperV1_0TargetTest /data/
```

## Run tests from PC
```
adb shell /data/VtsHalGatekeeperV1_0TargetTest
adb shell /data/VtsHalKeymasterV3_0TargetTest
```
Test for KeyMaster take quite long time, so you may be interested in running specific test only or exclude something (like long NewKeyGenerationTest.Rsa).
You can use following as example `adb shell /data/VtsHalKeymasterV3_0TargetTest --gtest_filter=*-NewKeyGenerationTest.Rsa`

General pattern to select/exclude tests:
```
--gtest_filter=POSTIVE_PATTERNS[-NEGATIVE_PATTERNS]
--gtest_filter=A.*:B.*-C.*:D.*
```

## Some troubleshooting
If you have all tests passed except
```
[  FAILED  ] 2 tests, listed below:
[  FAILED  ] AttestationTest.RsaAttestation
[  FAILED  ] AttestationTest.EcAttestation
```
quite possible that your board has optee-os built before November 2019. You can indirectly check this by build date reported for BL31. Like this:
```
[    0.215180] NOTICE:  BL31: Built : 15:31:45, Oct 30 2019
```
If this is it, update your loaders.
