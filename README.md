# Capsulev0.9.0-Memory-Config-Issue
This repo demonstrates the issue where upgrading to v0.9.0 causes Memory issues as they relate to project configuration.

**Software Versions Used:**
- Capsule v0.9.0
- CKB v0.107.0-rc1
- CKB-CLI v1.3.0
- Developer Training Course: https://github.com/jordanmack/developer-training-course
- Developer Training Course Script Examples https://github.com/jordanmack/developer-training-course-script-examples.git

**Issue Description: **

When executing a properly built Rust script using capsule v0.9.0, I get a MemOutOfBound error, and the script fails.  However, this same script will pass using Capsule v0.7.3 and identical settings.

**Issue Investigation: **

So far, it has been expressed that manipulating the parameters found in the cargo.toml file (in our case, represented here:https://github.com/jordanmack/developer-training-course-script-examples/blob/master/Cargo.toml#L4-L9), and using this (https://doc.rust-lang.org/rustc/codegen-options/index.html) as a reference, that I should be able to resolve the issues, based on past experience. Specifically, manipualating the codegen units was performed in the past and thought to be a fix.

I did notice this comment:"Beware, use difference heap size or memory block size may affect the verification result of the contract, some runtime errors such as out of memory may occur; you should always test the contract after customizing." taken from here: https://github.com/nervosnetwork/ckb-std and it seems quite related to the issue being experienced.  I also tried adjusting these values much higher, but no change resulted. 

![IMG_5434](https://user-images.githubusercontent.com/106935903/216389669-803a0fb5-a4cb-41b6-a4ba-36da5357439a.jpeg)

However, after exhausting the codegen unit options using 1, 16, 256, etc. turning optimizations on and off, and other various combinations, all that was seen were various other errors.  The 3 errors that were able to be produced were the following:

Test Passes with the following configuration and Capsule v0.7.3:
overflow-checks = true
opt-level = 's'
lto = false
codegen-units = 1
panic = 'abort'

![image](https://user-images.githubusercontent.com/106935903/216408100-f9f6cfa6-af58-4779-ba1a-c447600bf0eb.png)

Produced the "MemOutofBound" error with the following configuration and Capsule v0.9.0:
overflow-checks = true
opt-level = 's'
lto = false
codegen-units = 1
panic = 'abort'
![image](https://user-images.githubusercontent.com/106935903/216393587-ca316dad-0757-48e8-a775-4c3aee60b1c9.png)

Produced the "Invalid Instruction" error with the following configuration and Capsule v0.9.0:
overflow-checks = false
opt-level = 0
lto = false
codegen-units = 1
panic = 'abort'
![image](https://user-images.githubusercontent.com/106935903/216392420-14a4d586-d652-4b60-ba9a-13fcc9e30887.png)

"MemWriteOnExecutablePage" error (not produceable just yet. I saw and recorded the error, but don't recall the configuration that produced it)

**Steps To replicate:  (TBD--This isn't done.)**

1. Install CKB Testnet Node.
2. Install capsule v0.9.0.
3. Install Developer Training Course
5. Install Developer Training Course Script Examples
6. Run CKB
7. Run CKB Miner
8. Run CKB Indexer
9. Execute the Test Script (node index.js)
