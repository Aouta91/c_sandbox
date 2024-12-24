#day1 
```sh
sudo apt install gcc
sudo apt install build-essential 
sudo apt install build-essential 
sudo apt install libncurses6
sudo apt install libncurses-dev
sudo apt install flex
sudo apt install bison


 
```
```sh
cd /usr/src
 sudo tar -xvf ./linux-source-6.8.0.tar.bz2 
```
```
cd /usr/src/linux-source-6.8.0
```


Output by kernel 
```cpp
printk()
```
watch
```sh
journalxtl
#or
tail -f /var/log/kern.log
```

load module
```
sudo insmod ./hello.ko
```
```
sudo rmmod ./hello.ko
```

## paramdemo 
modinfo ./paramdemo.ko
```sh
sudo insmod paramdemo.ko m_cout=20 m_char='"test1 test2"'
 sudo insmod .build/paramdemo.ko m_cout=20 m_char='"test1 test2"'

modinfo ./paramdemo.ko
```

```sh
ls /sys/module/ | grep paramdemo
```
show params
```sh
cat /sys/module/paramdemo/parameters/*
```
## paramdemo2
```sh
modinfo .build/paramdemo.ko
sudo insmod .build/paramdemo.ko value1=31 value2=32

```

```sh
sudo /sys/module/paramdemo/parameters/value2
sudo su
echo 33 >  /sys/module/paramdemo/parameters/value2 
```
## task_1_random_generator
```sh
sudo insmod .build/paramdemo.ko min_of_range=31 max_of_range=32

sudo su
echo 90> /sys/module/mod_random_generator/parameters/min_of_range
echo 95 > /sys/module/mod_random_generator/parameters/min_of_range

echo 100 > /sys/module/mod_random_generator/parameters/max_of_range

```


# day2 
```sh
insmod ./.build/myalert.ko
cat /proc/kallsyms | grep myalert
insmod ./.build/hello.ko
```
Try to rm  myalert
```sh
rmmod myalert.ko # not work becase it use by hello
```
Firstly rm hello
```sh
rmmod hello.ko
rmmod myalert.ko
insmod ./.build/hello.ko
```
## Creta package
1. Insert *.ko to `misc` 
```sh
copy sources myalert.ko hello.ko to misc
cp unix_c_drivers/work/hello2.2_myalert/.build/*.ko /usr/lib/modules/6.8.0-38-generic/misc
```
2. Build dependencies 
```sh
sudo depmod
### load module from misc dir 
sudo modprobe hello
```
3. Check miss deps
```sh
sudo depmod -v | grep hello
```
4. remove deps
```sh
sudo modprobe -rf hello
sudo modprobe -rf myalert
```

## Hello 2.3

cat .build/Module.symvers