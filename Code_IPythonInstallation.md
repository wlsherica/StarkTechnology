#Steps to install ipython notebook on CDH 5.4

## Environment 

ps. Thanks to [Bryan's shell script](https://github.com/bryanyang0528/SparkTutorial/blob/cdh5.5/Installation/1_Python2.7.sh)

## Steps
Install packages you need later
```shell
sudo yum -y groupinstall "Development tools" && 
sudo yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel
```
Download py2.7.6
```shell
cd /opt
sudo wget --no-check-certificate https://www.python.org/ftp/python/2.7.6/Python-2.7.6.tar.xz
sudo tar xf Python-2.7.6.tar.xz
cd Python-2.7.6
sudo ./configure --prefix=/usr/local
sudo make && sudo make altinstall
```
Modify python's link
```shell
sudo ln -s /usr/local/bin/python2.7 /usr/local/bin/python
```
Echo python version
```shell
/usr/bin/python --version
```
Install pip
```shell
sudo wget https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py
sudo /usr/local/bin/python2.7 ez_setup.py
sudo /usr/local/bin/easy_install-2.7 pip
```
Install IPython notebook
```shell
sudo pip install ipython[all]
```
## Test
IPYTHON_OPTS="notebook --ip=0.0.0.0" ~/spark-1.4.1-bin-hadoop2.6/bin/pyspark

## In IPython notebook
Try it.
![get spark's sc](https://github.com/wlsherica/StarkTechnology/blob/master/pic/ipython_notebook.png)
