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

```

```



load module

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

## Radio (тестовое блочное устройство)
создание нового узла файловой системы
```sh
mknod /dev/radio c 240 0
# remove mode
rm -rf /dev/radio 
```
Попытка прочитать устройство ничем не закончится 
```sh 
cat /dev/radio

```

## task 2.1
```
 mknod /dev/gen_random_dev_demo c 239 0
 cat /dev/gen_random_dev_demo
tail /var/log/kern.log

```

# day 3
Radio v2

## radio v3  (with read implimentation)
```sh
mknod /dev/radio0 c 240 0
cat /dev/radio0 # This is radio speaking!

rmmod radio
cat /dev/radio0 # cat: /dev/radio0: Нет такого устройства или адреса
```

## radio v4
```sh
sudo rm -rf  /dev/radio*
mknod /dev/radio0 c 240 0
mknod /dev/radio1 c 240 1
mknod /dev/radio2 c 240 2
```
```sh
sudo chmod 666 /dev/radio*
sudo chmod 666 /dev/radio2

echo test  > /dev/radio2

echo test hello sdgfseg wsts gs test hello sdgfseg wsts gstest hello sdgfseg wsts gstest hello sdgfseg wsts gstest hello sdgfseg wsts gstest hello sdgfseg wsts gstest hello sdgfseg wsts gs > /dev/radio2


cat /dev/radio0
cat /dev/radio1
```

## radio v5 (task 3.2)

```sh
```

## procdemo (multythrade)

```sh
sudo insmod ./.build/procdemo.ko 
```

```sh
ls /proc/ | grep demo
```

cleanup
```sh
rmmod procdemo

```

# day 4

## mapdemo


## task 4.3 fakerandom

```sh

rmmod fakerandom
insmod ./.build/fakerandom.ko


```


```sh 

sudo rm -rf  /dev/fakerandom*

# mknod /dev/fakerandom0 c 239 0
mknod /dev/fakerandom1 c 240 1
mknod /dev/fakerandom2 c 240 2
```
```sh
sudo chmod 666 /dev/fakerandom*
sudo chmod 666 /dev/fakerandom2

echo test  > /dev/fakerandom2

# echo test hello sdgfseg wsts gs test hello sdgfseg wsts gstest hello sdgfseg wsts gstest hello sdgfseg wsts gstest hello sdgfseg wsts gstest hello sdgfseg wsts gstest hello sdgfseg wsts gs > /dev/fakerandom2

echo test hello > /dev/fakerandom2

# cat /dev/fakerandom0
cat /dev/fakerandom1
```

# Day5

## cblk 
```sh
lsblk -f
 mkfs -t ext4 /dev/sblkdev1
 mount -o defaults /dev/sblkdev1  /mnt/block
 lsblk -f # check mount
#  cd /mnt/block
 
 mkdir /mnt/block/test
 echo "hello txt" > /mnt/block/test/hello.txt

 lsblk -f # check utilysation
```