Some good links on debian and packaging, I will keep updating it as I find interesting in this topic

https://www.theo-andreou.org/?p=1112#toc-apply-patches-for-policy-compliance

http://blog.packagecloud.io/debian/debuild/packaging/2015/06/08/buildling-deb-packages-with-debuild/
http://blog.packagecloud.io/eng/2014/10/28/howto-gpg-sign-verify-deb-packages-apt-repositories/
http://santi-bassett.blogspot.in/2014/07/how-to-create-debian-package.html

debian:
https://www.debian.org/doc/manuals/maint-guide/build.en.html
https://www.debian.org/doc/manuals/packaging-tutorial/packaging-tutorial.en.pdf


ubuntu:
https://help.launchpad.net/Packaging/PPA/BuildingASourcePackage
https://help.launchpad.net/Packaging/PPA/Uploading
http://packaging.ubuntu.com/html/
https://help.ubuntu.com/community/LocalAptGetRepository

contribution:
https://raphaelhertzog.com/contributing-to-debian/


BTS:


Contrib:
https://mentors.debian.net/intro-maintainers 


pbuilder
========
[pbuilder manpage](http://manpages.ubuntu.com/manpages/precise/man8/pbuilder.8.html)
[builder deb packages with pbuilder](https://blog.packagecloud.io/eng/2015/05/18/building-deb-packages-with-pbuilder/)
[packaging with git](https://people.debian.org/~debalance/packaging-with-git.html)
[debian packages in git](https://honk.sigxcpu.org/piki/development/debian_packages_in_git/)
[man gbp](http://honk.sigxcpu.org/projects/git-buildpackage/manual-html/man.gbp.pq.html)


gbp import-dsc

  sudo pbuilder create --distribution squeeze --mirror \
  ftp://ftp.us.debian.org/debian/ --debootstrapopts \
  "--keyring=/usr/share/keyrings/debian-archive-keyring.gpg"

  sudo pbuilder build *.dsc
  sudo pbuilder --login
  sudo pdebuild --use-pdebuild-internal

DIST=jessie ARCH=amd64 pbuilder build --debbuildopts "-sa -v1.0" foo.dsc



http://linux.lsdev.sil.org/wiki/index.php/Packaging_using_gbp


