# Maintainer: Simon Legner <Simon.Legner@gmail.com>
# Maintainer: Shengyu Zhang <la@archlinuxcn.org>
pkgname=coredns
pkgver=1.11.1
pkgrel=1
pkgdesc="A DNS server that chains plugins"
makedepends=('go' 'make')
conflicts=('coredns-bin')
arch=('i686' 'pentium4' 'x86_64' 'arm' 'armv7h' 'armv6h' 'aarch64')
url="https://github.com/coredns/coredns"
license=('Apache')
provides=('coredns')
source=(coredns-${pkgver}.tar.gz::https://github.com/coredns/${pkgname}/archive/v${pkgver}.tar.gz
coredns.service
coredns-sysusers.conf)

sha256sums=('4e1cde1759d1705baa9375127eb405cd2f5031f9152947bb958a51fee5898d8c'
            '030cd8e938c293c11a9acdb09b138f98b37874772072336792ec4bf0d9eff9b1'
            '536d03f8b20b0d2d6e8f96edd7e4e4dd7f6fef39ab0e952522d8725f3cc186b7')

build() {
  export GOPATH="$srcdir/build"
  export PATH=$GOPATH/bin:$PATH
  cd coredns-$pkgver

  # Remove unneeded plugins
  sed -i '/azure:azure/d' plugin.cfg
  sed -i '/chaos:chaos/d' plugin.cfg
  sed -i '/clouddns:clouddns/d' plugin.cfg
  sed -i '/dns64:dns64/d' plugin.cfg
  sed -i '/dnssec:dnssec/d' plugin.cfg
  sed -i '/dnstap:dnstap/d' plugin.cfg
  sed -i '/erratic:erratic/d' plugin.cfg
  sed -i '/etcd:etcd/d' plugin.cfg
  sed -i '/file:file/d' plugin.cfg
  sed -i '/geoip:geoip/d' plugin.cfg
  sed -i '/grpc:grpc/d' plugin.cfg
  sed -i '/health:health/d' plugin.cfg
  sed -i '/k8s_external:k8s_external/d' plugin.cfg
  sed -i '/kubernetes:kubernetes/d' plugin.cfg
  sed -i '/loadbalance:loadbalance/d' plugin.cfg
  sed -i '/nsid:nsid/d' plugin.cfg
  sed -i '/pprof:pprof/d' plugin.cfg
  sed -i '/ready:ready/d' plugin.cfg
  sed -i '/prometheus:metrics/d' plugin.cfg
  sed -i '/root:root/d' plugin.cfg
  sed -i '/route53:route53/d' plugin.cfg
  sed -i '/sign:sign/d' plugin.cfg
  sed -i '/secondary:secondary/d' plugin.cfg
  sed -i '/trace:trace/d' plugin.cfg
  sed -i '/transfer:transfer/d' plugin.cfg
  sed -i '/tsig:tsig/d' plugin.cfg

  # Add custom plugins
  echo "iface:github.com/Douile/coredns-iface" >> plugin.cfg
  echo "virt:github.com/Douile/coredns-libvirt" >> plugin.cfg

  make coredns
}

package() {
  install -Dm755 "$srcdir/coredns-$pkgver/coredns" "$pkgdir/usr/bin/coredns"
  install -Dm644 "$srcdir/coredns.service" "$pkgdir/usr/lib/systemd/system/coredns.service"
  install -Dm644 "$srcdir/coredns-sysusers.conf" "$pkgdir/usr/lib/sysusers.d/coredns.conf"
  install -d "${pkgdir}/etc/coredns"
}
