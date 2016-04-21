
pkgname=dkms
pkgver=2.2.0.3+git151023
pkgrel=1
pkgdesc='Dynamic Kernel Module Support'
arch=('x86_64')
url='http://linux.dell.com/dkms/'
license=('GPL2')
depends=('bash' 'kmod' 'gcc' 'make' 'patch')
makedepends=('git')
optdepends=('linux-headers')
backup=('etc/dkms/framework.conf')
install=$pkgname.install
source=('git+git://linux.dell.com/dkms.git#commit=7b6e78f'
        '02-no-kernel-hook.patch'
        'hook.install'
        'hook.remove'
        'hook.sh')
md5sums=('SKIP'
         '82d520c39c99c34977e48b313a189c6c'
         'a8a715872817f48250e8bdb41d15ce2d'
         '0169b904e945165e0122fa0791fa7405'
         '8a418fa22fae257c4730c45400610a90')

prepare() {
  cd dkms

  # apply patch from the source array (should be a pacman feature)
  local filename
  for filename in "${source[@]}"; do
    if [[ "$filename" =~ \.patch$ ]]; then
      msg2 "Applying patch $filename"
      patch -p1 -N -i "$srcdir/$filename"
    fi
  done

#   # /usr move
#   msg2 '/usr move patching'
#   for i in dkms{,_framework.conf,.bash-completion,.8,_common.postinst}; do
#     sed -ri 's,/lib/modules,/usr/lib/modules,g' "$i"
#   done
}

package() {
  # alpm hook
  install -D -m 644 hook.install "$pkgdir/usr/share/libalpm/hooks/70-dkms-install.hook"
  install -D -m 644 hook.remove "$pkgdir/usr/share/libalpm/hooks/70-dkms-remove.hook"
  install -D -m 755 hook.sh "$pkgdir/lib/dkms/alpm-hook"
  # upstream installer
  cd dkms
  make \
    DESTDIR="$pkgdir" \
    SBIN="$pkgdir/usr/bin" \
    BASHDIR="$pkgdir/usr/share/bash-completion/completions" \
    install
}

# vim:set ts=2 sw=2 et:
