
release checklist
    update doc/Makefile.am list: cd pd/doc;
        (find . -type f | sort | awk '{print "    ", $1, "\\"}';\
            echo '     $(empty)') > /tmp/foo.txt
    version string in ../src/m_pd.h ../configure.ac ../src/pd.rc
    release notes ../doc/1.manual/x5.htm
    copyright date in ../README.txt
    git commit -a
    test compilation on linux/msw/mac as follows:
    cd linux; ./make-release 0.35-0  or 0.35-test11, etc
        ... compile on MAC:
            first build POs on linux because I can't install gettext on mac:
            ./autogen.sh; ./configure --enable-jack; make; rsync -avzl po/ <mac>:msp/build/po/
            scp source tarball to Mac and unpack in ~/build.
            in ~/build: build-autotools and build-ppc
            in ~/b32: build-i386
            scp tarballs back to linux
        ... compile on windows:
            cd msw
            ./build-msw-64.sh <version>
            ./build-wxp-32.sh <version>
    git tag | tail (to see existing tags)
    git tag 0.43-3test1 (e.g.)
    git push origin
    git push origin --tags
        ... (I don't use 'mirror' here because afraid of deleting PR branches)
    git push sourceforge --mirror
    copy from ~/pd/dist to ~/bis/lib/public_html/Software/
    rsync -avzl --delete ~/pd/doc/1.manual/ \
        ~/bis/lib/public_html/Pd_documentation/
    chmod -R go-w ~/bis/lib/public_html/Pd_documentation/
    cp -a ~/pd/README.txt ~/bis/lib/public_html/Software/pd-README.txt
    (cd /home/msp/bis/lib/public_html/Software; htmldir.perl .)
    nedit-client /home/msp/bis/lib/public_html/software.htm
    copy-out.sh +

    mail release notice from /home/msp/pd/attic/pd-announce

rpm building (inactive)
    update rpmspec version number
    as root:
    rpmbuild -ba rpmspec
    rpmbuild -bb rpmspec-alsa
    check size of compressed files:
        /usr/src/redhat/SRPMS/pd-0.36-0.src.rpm
        /usr/src/redhat/RPMS/i386/pd-0.36-0.i386.rpm 
        /usr/src/redhat/RPMS/i386/pd-alsa-0.36-0.i386.rpm
    copy from /usr/src/redhat/RPMS/i386 and /usr/src/redhat/SRPMS
