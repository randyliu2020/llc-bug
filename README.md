# llc-bug

my source code is add.cl,the code is

kernel void add(global int* a, global int *b) {
	int gid = get_global_id(0);
	b[gid] = a[gid] + a[gid];
}

I use command:
clang -c -x cl -emit-llvm -S -cl-std=CL2.0 -Xclang -finclude-default-header add.cl -o add.bc

then use:
llc -march=riscv32 -filetype=obj add.bc -o add.o

but it reports me some bugs,like

LLVM ERROR: RV64 target requires an RV64 CPU
PLEASE submit a bug report to https://bugs.llvm.org/ and include the crash backtrace.
Stack dump:
0.	Program arguments: llc -march=riscv64 -filetype=obj add.bc -o add.o
1.	Running pass 'Function Pass Manager' on module 'add.bc'.
2.	Running pass 'Expand Atomic instructions' on function '@add'
 #0 0x000055fcbfe92334 PrintStackTraceSignalHandler(void*) Signals.cpp:0:0
 #1 0x000055fcbfe8fb4e SignalHandler(int) Signals.cpp:0:0
 #2 0x00007fa7e6dbe3c0 __restore_rt (/lib/x86_64-linux-gnu/libpthread.so.0+0x153c0)
 #3 0x00007fa7e68ab18b raise /build/glibc-eX1tMB/glibc-2.31/signal/../sysdeps/unix/sysv/linux/raise.c:51:1
 #4 0x00007fa7e688a859 abort /build/glibc-eX1tMB/glibc-2.31/stdlib/abort.c:81:7
 #5 0x000055fcbfdea5fa llvm::report_fatal_error(llvm::Twine const&, bool) (/usr/local/bin/llc+0x14285fa)
 #6 0x000055fcbfdea79e (/usr/local/bin/llc+0x142879e)
 #7 0x000055fcbee13b7d (/usr/local/bin/llc+0x451b7d)
 #8 0x000055fcbee0aaf2 llvm::RISCVSubtarget::initializeSubtargetDependencies(llvm::Triple const&, llvm::StringRef, llvm::StringRef, llvm::StringRef, llvm::StringRef) (/usr/local/bin/llc+0x448af2)
 #9 0x000055fcbee0b66b llvm::RISCVSubtarget::RISCVSubtarget(llvm::Triple const&, llvm::StringRef, llvm::StringRef, llvm::StringRef, llvm::StringRef, llvm::TargetMachine const&) (/usr/local/bin/llc+0x44966b)
#10 0x000055fcbed9a598 llvm::RISCVTargetMachine::getSubtargetImpl(llvm::Function const&) const (/usr/local/bin/llc+0x3d8598)
#11 0x000055fcbf0f6bd4 (anonymous namespace)::AtomicExpand::runOnFunction(llvm::Function&) AtomicExpandPass.cpp:0:0
#12 0x000055fcbf696bc0 llvm::FPPassManager::runOnFunction(llvm::Function&) (/usr/local/bin/llc+0xcd4bc0)
#13 0x000055fcbf696d39 llvm::FPPassManager::runOnModule(llvm::Module&) (/usr/local/bin/llc+0xcd4d39)
#14 0x000055fcbf699070 llvm::legacy::PassManagerImpl::run(llvm::Module&) (/usr/local/bin/llc+0xcd7070)
#15 0x000055fcbed2a1ef main (/usr/local/bin/llc+0x3681ef)
#16 0x00007fa7e688c0b3 __libc_start_main /build/glibc-eX1tMB/glibc-2.31/csu/../csu/libc-start.c:342:3
#17 0x000055fcbed769ce _start (/usr/local/bin/llc+0x3b49ce)
Aborted (core dumped)
