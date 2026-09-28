# Local package recipe for the official Cursor CLI binary archive.
pkgname=cursor-cli
_upstream_ver='2026.09.26-dd393fe'
pkgver=2026.09.26.1.dd393fe
pkgrel=1
epoch=1
pkgdesc='Cursor Agent CLI - AI-powered coding assistant (official binary)'
arch=('x86_64')
url='https://cursor.com'
license=('LicenseRef-Cursor')
depends=('bash' 'gcc-libs' 'glibc' 'zlib')
provides=('cursor-agent')
options=('!strip')
source=('LICENSE')
source_x86_64=("cursor-cli-${_upstream_ver}-x86_64.tar.gz::https://downloads.cursor.com/lab/${_upstream_ver}/linux/x64/agent-cli-package.tar.gz")
sha256sums=('9f2174c1ec04f0e9038f7d02dce70f2163855d72423f9be45fbbe0c3aa73bd54')
sha256sums_x86_64=('8085fd120f5c71f4eae7fea26a043718e5644e3071e4fab3220a0e58c51f9593')

package() {
  install -d "${pkgdir}/opt/cursor-agent" "${pkgdir}/usr/bin"
  cp -a "${srcdir}/dist-package/." "${pkgdir}/opt/cursor-agent/"

  # Keep updates under pacman control instead of allowing the CLI to install
  # another copy into ~/.local. This is a supported, hidden Cursor CLI option.
  local launcher="${pkgdir}/opt/cursor-agent/cursor-agent"
  if ! grep -Fq '"$SCRIPT_DIR/index.js" "$@"' "${launcher}"; then
    printf '%s\n' 'error: upstream cursor-agent launcher changed; review the auto-update patch' >&2
    return 1
  fi
  sed -i 's|"$SCRIPT_DIR/index.js" "$@"|"$SCRIPT_DIR/index.js" --disable-auto-update "$@"|g' "${launcher}"

  ln -s /opt/cursor-agent/cursor-agent "${pkgdir}/usr/bin/agent"
  ln -s /opt/cursor-agent/cursor-agent "${pkgdir}/usr/bin/cursor-agent"

  install -Dm644 "${srcdir}/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
