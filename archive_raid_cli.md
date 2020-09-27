https://www.server-world.info/en/note?os=Ubuntu_16.04&p=lvm&f=4
https://www.slackwiki.com/LVM/Luks_Encryption
https://linuxgazette.net/140/pfeiffer.html
https://wiki.archlinux.org/index.php/Partitioning#GNU_Parted
https://raid.wiki.kernel.org/index.php/A_guide_to_mdadm
https://www.cyberciti.biz/security/howto-linux-hard-disk-encryption-with-luks-cryptsetup-command/

    
    
    6  sudo apt update
    7  sudo apt-get install -y mdadm lvm2

   84  mdadm --detail /dev/md/2
   85  mdadm /dev/md/2 -a /dev/sdd
 
   87  apt install smartmontools
   88  smartctl -i /dev/sdd
   89  smartctl -i short /dev/sdd


   95  mdadm --detail /dev/md/2

   98  umount /nas
   99  mdadm --stop /dev/md/2
  100  vgchange -a n vg1
  101  mdadm --stop /dev/md/2

  104  mdadm --assemble --force --no-degraded /dev/md/2 /dev/sdc /dev/sdd
  105  mdadm --assemble --force --no-degraded /dev/md/2 /dev/sdc1 /dev/sdd1
  106  lsblk
  107  mdadm --assemble --force --no-degraded /dev/md/2 /dev/sdc3 /dev/sdd3
  108  mdadm --assemble --force --no-degraded /dev/md/2 /dev/sdc2 /dev/sdd2
  109  mdadm --assemble --force --no-degraded --scan 
  110  mdadm --detail /dev/md/2
  111  mdadm /dev/md/2 -a /dev/sdd
  112  mdadm /dev/md/2 -a /dev/sdd1
  113  ls -l /dev/sdd
  114  man mdadm
  115  history
  116  mdadm /dev/md/2 -a /dev/sdd
  117  lslbk
  118  lsblk
  119  mdadm --detail /dev/md/2

  121  mdadm --stop /dev/md/2
  122  vgchange -a n vg1
  123  mdadm --stop /dev/md/2
  124  lsblk
  125  dmsetup ls

  127  dmsetup info /dev/dm-4

  129  pvs

  131  mdadm --assemble --force --no-degraded --scan

  134  vgchange -a n vg1
  135  mdadm --stop /dev/md/2



  403  cd /
  404  umount /nas
  405  vgchange -a n vg1
  406  mdadm --stop /dev/md2
  407  mdadm --detail /dev/md/2
  408  vgdisplay
  409  mdadm --assemble /dev/md/2 /dev/sdc3
  410  mdadm --detail /dev/md/2
  411  mdadm --assemble --run /dev/md/2 /dev/sdc3
  412  mdadm --assemble --run /dev/md/2 /dev/sdc
  413  mdadm --assemble --run /dev/md/2 /dev/sdc2
  414  mdadm --assemble --run /dev/md/2 /dev/sdc1
  415  mdadm --detail /dev/md/2
  416  umount /nas
  417  vgdisplay


  922  history | grep mount
  923  mount /dev/sdb1 /backup


  935  man fdisk
  936  fdisk -l /dev/sda

  955  cfdisk /dev/sdc
  956  cfdisk /dev/sdd
  

  962  mdadm --create /dev/md/archive /dev/sdc1 /dev/sdd1 --level=1 --raid-devices=2
  966  cat /proc/mdstat


  987  cryptsetup -c aes-cbc-essiv:sha256 -y -s 256 luksFormat /dev/md/llyr:archive


  990  cryptsetup luksOpen /dev/md/llyr\:archive crypt_archive

  992  pvcreate /dev/mapper/crypt_archive 
  993  vgcreate -v crypt_archive_vg /dev/mapper/crypt_archive 

  995  pvscan
  997  lvcreate -l 100%FREE -n archivelv crypt_archive_vg

 1006  cryptsetup -v status crypt_archive_vg-archivelv
 1007  cryptsetup -v status crypt_archive
 1008  mkfs.ext4 /dev/mapper/crypt_archive_vg-archivelv 

 1011  mkdir /archive
 1012  mount /dev/mapper/crypt_archive_vg-archivelv /archive

 1015  cryptsetup -v status crypt_archive


root@llyr:/archive/media# ls -l /dev/mapper
total 0
crw------- 1 root root 10, 236 Sep 23 22:22 control
lrwxrwxrwx 1 root root       7 Sep 23 22:33 crypt_archive -> ../dm-3
lrwxrwxrwx 1 root root       7 Sep 23 22:40 crypt_archive_vg-archivelv -> ../dm-4
lrwxrwxrwx 1 root root       7 Sep 23 22:22 sda6_crypt -> ../dm-0
lrwxrwxrwx 1 root root       7 Sep 23 22:22 vgubuntu-root -> ../dm-1
lrwxrwxrwx 1 root root       7 Sep 23 22:22 vgubuntu-swap_1 -> ../dm-2
