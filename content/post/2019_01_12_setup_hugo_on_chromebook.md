---

title: "Setting up Hugo on Chrome OS (Pixel Slate)"

date: 2019-01-12T12:45:00+09:00

tags: [crostini, chromeos, hugo, slate]

categories: [note, howto]

---



## The Background

**If you don't want to read my novel on this then skip to the last part.**



I just got my new Pixel Slate (about almost a month and a half late), and my impression is overall pretty good. I have not used it as my main device yet because I'm still in a middle of an on going project and that I still need to debug on iPhone. However, maybe sometime next month, I have to send in my Macbook for a keyboard repair so maybe I can review the device then.



## The Contents

So the first thing I want to do with the Slate is that I want to try to write some blog post at Starbucks, something that it is hard to do with the giant 15" Macbook Pro.



### 1) Turn on the Linux stuff

I can do it the normal way by going to the setting and turn it on, but I might as well try something new with the Pixel Slate like saying `Ok Google, open the setting menu for me`. Surprisingly, `Google Assistant` is actually smart enough to open the Setting widow for me, and I just have to type `Linux` in the search bar to turn it on. The Slate downloaded something and I have a working terminal, which I just do



```bash

sudo apt-get update

```



There are no sudo password (no idea what is going on). After that then I just went to install the `zsh` and set it to my liking.



### 2) Install VSCode

Installing VSCode is pretty straight forward that I can just go to the official site and download the `.deb` file. It appears in the ChromeOS's File app. To install it, I just have to drag the `.deb` file to the Linux tab and drop it there. ChromeOS will copy the `.deb` file to the Crostini's home directory. The great thing about this is that I can just right-click on the `.deb` file in the Linux tab and just click on `Install with Linux (beta)`. After the installation, ChromeOS will even make an icon in the app drawer for us, which is great!



### 3) Install Hugo

This is actually the more complicated part. Since we are dealing with Linux here, the sensible answer to the OSX `brew` is the `apt-get` so I did the



```bash

sudo apt-get update

sudo apt-get install hugo   // This was a mistakem which you will see why later so don't do this.

```



And it actually works!(Not really, so don't do the `apt-get install hugo`) I can run `hugo version` no problem. So I just went ahead and clone my blog repository and just run `hugo server` and it should work. Unfortunately, things aren't that easy since I cannot access the `1313` port from outside crostini. I can reach `localhost:8080` if I set the port to `8080` using `hugo server -p 8080`. I did some searching and found that you can access any port in crostini using the `penguin,linux.text:` domain and tried it, but that domain still does not work. I did some searching and found that Hugo will bind to 127.0.0.1, so it work if I use



```bash

hugo server --bind 0.0.0.0

```



and it works, I can access the site from the Chrome outside of Crostini. However, problems remain that the css files are not found `404` and the site is messy. I checked Chrome developer console and found that the css files request were for `localhost` and not the `penguin.test`. So that I did is I end up just mounting it to `8080` using



```bash

hugo server -p 8080

```



and it just works. Note that you can actually mount it to other ports too as listed [here](https://www.reddit.com/r/Crostini/comments/99s3t9/wellknown_ports_are_now_autoforwarded_to_the/?st=jmbaafo7&sh=1c22ec8e). So any of these ports should allow you to connect via just `localhost:<port>`



```bash

// TCP ports to statically forward to the container over SSH.

const uint16_t kStaticForwardPorts[] = {

  3000,  // Rails

  4200,  // Angular

  5000,  // Flask

  8000,  // Django

  8008,  // HTTP alternative port

  8080,  // HTTP alternative port

  8085,  // Cloud SDK

  8888,  // ipython/jupyter

  9005,  // Firebase login

};

```



The last problem remains is that the site is still all messed up even with the CSS. I thought there were some problem with my blog markdown so I spent like 3 hours debugging without much success. It seems like that the block function of hugo does not work. I reinstall Hugo several times without much success. Until I see that the Hugo that came with the `apt-get install hugo` was version `0.18`. I checked the current hugo version and it's like `0.53`. So it's just that the package manager will not provide the newest version for ChromeOS and I was stuck with version `0.18` where code block and block do not work. I tried several ways of installing the newest version (without building it my self) like `linuxbrew` (which I think is great but did not work in this case). So what I end up doing is heading to Hugo site and just download the `.deb` file and just right-click and install it like VSCode and now it works!


## Summary

1. Enable Linux in Chrome OS in the setting

2. Install VSCode by downloading the `.deb` file and drag it into linux container in ChromeOS's File app and right-click to install it

3. Clone the blog from github

4. Install the latest version of Hugo by going to their site and download the `.deb` file for amd64 and drag it into linux container and right-click to install it

5. run hugo on port '8080' or any other common ports listed above.