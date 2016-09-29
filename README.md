# zhuyong.me

## Setup Environment On New Computer

1. Download hugo binary file from : https://github.com/spf13/hugo/releases
2. Install Pygments `sudo pip install Pygments`
3. Install https://github.com/john2x/solarized-pygment.git
    $ git clone https://github.com/john2x/solarized-pygment.git
    $ cd solarized-pygment
    $ sudo python setup.py install
4. Clone this repository
	$ git clone -b source https://github.com/yongzhy/yongzhy.github.io.git
	$ cd yongzhy.github.io.git
	$ git subtree pull --prefix=public https://github.com/yongzhy/yongzhy.github.io.git master
5. Add content
    $ hugo new post/newpost-title
7. Generate And Publish
    $ hugo
    $ bash deploy.sh