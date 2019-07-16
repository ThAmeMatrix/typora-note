# 树莓派安装miniconda

```shell
wget http://repo.continuum.io/miniconda/Miniconda3-latest-Linux-armv7l.sh
sudo md5sum Miniconda3-latest-Linux-armv7l.sh # (optional) check md5
bash Miniconda3-latest-Linux-armv7l.sh # -> change default directory to /home/pi/miniconda3
sudo nano /home/pi/.bashrc # -> add: export PATH="/home/pi/miniconda3/bin:$PATH"
sudo reboot -h now
conda 
python --version
sudo chown -R pi miniconda3
```

