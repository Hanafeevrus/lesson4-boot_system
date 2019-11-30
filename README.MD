 # lesson4-Boot_system
попасть в систему без пароля
#### 1 способ  init=/bin/sh
изменяем параметры загрузки контекстного меню выбора загрузки ядра в GUI  
   - удаляем параметр загрузки ядра `console=ttyS0,115200n8`
   - добавляем параметры `selinux=0` `init=/bin/sh`  
   #### 2 способ  rd.break  
   изменяем параметры загрузки контекстного меню выбора загрузки ядра в GUI  
   - удаляем параметр загрузки ядра `console=ttyS0,115200n8`
   - добавляем параметры `selinux=0` `rd.break`  
 после загрузки систему можно изменить пароль администратора  
   ` mount -o remount,rw /sysroot`  
   `chroot /sysroot`  
   `passwd root` вводим пассворд с подтверждением и проверяем перезагрузившись  
   #### 3 способ  rw init=/sysroot/bin/sh
   изменяем параметры загрузки контекстного меню выбора загрузки ядра в GUI  
   - удаляем параметр загрузки ядра `console=ttyS0,115200n8`
   - добавляем параметры `selinux=0` `init=/sysroot/bin/sh`  
   - изменяем `ro` на `rw` 
   
   #### Установить систему с LVM и переименовать VG
   * Проверяем состояние системы командой `vgs`  
   VG       #PV #LV #SN Attr   VSize   VFree  
   VolGroup00   1   2   0 wz--n- <38.97g    0  
   * переименовываем VolGroup  
   vgrename VolGroup00 OtusRoot  
   далее меняем значения в файлах `/etc/fstab` `/etc/default/grub` `/boot/grub2/grub.cfg`  
   выполняем  
   `mkinitrd -f -v /boot/initramfs-$(uname -r).img $(uname -r)`  
   * перезагружаемся и проверяем `vgs`  
   VG       #PV #LV #SN Attr   VSize   VFree  
   OtusRoot   1   2   0 wz--n- <38.97g    0
   
     
   #### добавление модуля в загрузку
   создаем папку `/usr/lib/dracut/modules.d/01test` с файлами `test.sh` и `modules-setup.sh`  
   Пересобираем образ initrd  
   `dracut -f -v`  
   редактируем `grub.cfg`, удаляем параметры загрузки `rghb` и `quiet`  
   после перезагрузки увидим отображение пингвина в терминале.  
   ###### < I'm dracut module >
      > -------------------
      > \
     > \
        .--.
       |o_o |
       |:_/ |
      //   \ \
     (|     | )
    /'\_   _/`\
    \___)=(___/
