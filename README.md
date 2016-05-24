> I do not own any of this, This is just an updated guide on how to install openALPR on a raspberry pi
# links to all the github pages of the actual content owners
[link](https://github.com/openalpr/openalpr)
[link](https://github.com/tesseract-ocr/tesseract)
[link](https://github.com/tesseract-ocr/tessdata)
[link](https://github.com/Itseez/opencv)
[link](https://github.com/DanBloomberg/leptonica)
##If you just imaged the Raspberry Pi:
    sudo apt-get update
	sudo apt-get upgrade
	sudo rpi-update

	sudo raspi-config 
	* enable camera
	* enable ssh (if its not already enabled)
	* expand file system (you might have to do the steps before this, then reboot, then expand file system, then hit ok and it wil reboot again)
/*********************************************************/
---
## Install Dependencies: (I believe these are all current and up to date)
	sudo apt-get install autoconf automake libtool
	sudo apt-get install libpng12-dev
	sudo apt-get install libjpeg62-turbo-dev
	sudo apt-get install libtiff5-dev
	sudo apt-get install zlib1g-dev
	sudo apt-get install git-core
	sudo apt-get install cmake
	sudo apt-get install libcurl4-openssl-dev liblog4cplus-1.0-4 liblog4cplus-dev uuid-dev
	sudo apt-get install libjasper-dev 
	sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
	sudo apt-get install libatlas-base-dev gfortran
---
/**********************************************************/
## this method will put everything opencv/alpr related into one folder:
	cd ~
	wget https://github.com/openalpr/openalpr/archive/master.zip
	unzip master.zip
	cd openalpr-master
	sudo mkdir libraries
	cd libraries
	sudo wget https://github.com/tesseract-ocr/tesseract/archive/master.zip
	(this is the tesseract library)
	sudo wget https://github.com/tesseract-ocr/tessdata/archive/master.zip
	(this is the tesseract laguage library)
	sudo wget https://github.com/Itseez/opencv/archive/master.zip
	(opencv library)
	sudo wget http://www.leptonica.com/source/leptonica-1.73.tar.gz
	(leptonica library)
---
/***********************************************************/
## now unzip all master.zip files:
	sudo unzip master.zip
	sudo unzip master.zip.1
	sudo unzip master.zip.2
	sudo tar –xvf leptonica-1.73.tar.gz
---

/*************************************************************/
## compile leptonica:
	cd leptonica-1.73
	sudo ./configure
	sudo make
	sudo make install
	sudo ldconfig

## compile tesseract:
    cd ~/openalpr-master/libraries /tesseract-master
	sudo ./autogen.sh
	sudo ./configure
	sudo make
	sudo make install
	sudo ldconfig

## compile opencv:
	cd ~/openalpr-master/libraries/opencv-master
	mkdir release (it doesn't matter what you name the directory)
	cd release
	sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..
	sudo make
	sudo make install
	sudo ldconfig

## add to your .bashrc file:
	cd ~ 
	sudo nano .bashrc
	(at the very end of file add this)
	export TESSDATA_PREFIX=/home/pi/openalpr-master/libraries/tessdata-master
	(I believe this step needs a reboot to take effect, so do that just to be safe)

	cd ~/openalpr-master/src 
	sudo nano CMakeLists.txt
	(delete the second if statement, starts at line 22, so delete line 22,23,24 and replace it with these two lines)
	SET(OpenCV_DIR "/usr/local/lib") or SET(OpenCV_DIR "~/openalpr-master/libraries/opencv-master/release/lib")
	SET(Tesseract_DIR "/home/pi/openalpr-master/libraries/tesseract-master")
	/* to save
	Ctrl x
	y
	enter
	*/

## Compile openALPR:
	sudo cmake ./
	sudo make
	sudo make install
	sudo ldconfig
---
/*************************************************************/

## time to test the library:
	cd ~
	wget http://plates.openalpr.com/ea7the.jpg
	alpr -c us ea7the.jpg

	result: 
	plate0: 10 results
		- EA7THE	 confidence: 92.4795
		- EA7TBE	 confidence: 84.0421
		- EA7TRE	 confidence: 83.1932
		- EA7TE	 confidence: 82.0527
		- EA7T8E	 confidence: 81.7845
		- EA7TME	 confidence: 80.8062
		- EA7THB	 confidence: 76.6468
		- EA7TH6	 confidence: 76.6153
		- EA7TH	 confidence: 75.2232
		- EA7TBB	 confidence: 68.2095
