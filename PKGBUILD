# Maintainer: Xilin Wu <sophon@radxa.com>

pkgbase=qcom-qairt
pkgname=('qcom-qairt-sdk-v68' 'qcom-qairt-sdk-v73')
pkgver=2.50.40.260831
pkgrel=1
arch=('aarch64')
url="https://softwarecenter.qualcomm.com"
license=('custom:Qualcomm-Technologies-Inc.-Proprietary')
makedepends=('unzip')
options=('!strip' '!debug')

_platform_dir='aarch64-oe-linux-gcc11.2'
_python_platform_dir='linux-aarch64-oe-gcc11.2'

source=("https://softwarecenter.qualcomm.com/api/download/software/sdks/Qualcomm_AI_Runtime_Community/All/${pkgver}/v${pkgver}.zip")
sha256sums=('e821833cff67b0c55a413c920658cf9132a92e06309b6c26900149fd1a8809da')

_package_qairt_sdk() {
    local hexagon_dir=$1
    local hexagon_version=${hexagon_dir#hexagon-v}
    local _sdk_dir="${srcdir}/qairt/${pkgver}"
    local library
    local python_tool

    install -dm755 \
        "${pkgdir}/usr/bin" \
        "${pkgdir}/usr/include" \
        "${pkgdir}/usr/lib" \
        "${pkgdir}/usr/lib/qcom-qairt/python" \
        "${pkgdir}/usr/lib/rfsa/adsp" \
        "${pkgdir}/usr/share" \
        "${pkgdir}/usr/share/licenses/${pkgname}"

    cp -a "${_sdk_dir}/bin/${_platform_dir}/." "${pkgdir}/usr/bin/"
    cp -a "${_sdk_dir}/include/." "${pkgdir}/usr/include/"
    for library in "${_sdk_dir}/lib/${_platform_dir}/"*; do
        [[ ${library##*/} == *V[0-9]* && ${library##*/} != *V${hexagon_version}* ]] && continue
        install -m755 "${library}" "${pkgdir}/usr/lib/"
    done
    cp -a "${_sdk_dir}/lib/${hexagon_dir}/unsigned/." "${pkgdir}/usr/lib/rfsa/adsp/"
    cp -a "${_sdk_dir}/lib/python/." "${pkgdir}/usr/lib/qcom-qairt/python/"
    cp -a "${_sdk_dir}/share/." "${pkgdir}/usr/share/"

    # The archive includes x86 and Windows extension modules alongside ARM64.
    find "${pkgdir}/usr/lib/qcom-qairt/python" -type f \( -name '*.so' -o -name '*.pyd' \) -delete
    while IFS= read -r -d '' library; do
        install -Dm755 "${library}" "${pkgdir}/usr/lib/qcom-qairt/python/${library#"${_sdk_dir}/lib/python/"}"
    done < <(find "${_sdk_dir}/lib/python" -type f -path "*/${_python_platform_dir}/*.so" -print0)

    for python_tool in qairt-accuracy-debugger qairt-converter qairt-dlc-diff qairt-dlc-info qairt-quantizer; do
        sed -i '1c #!/usr/bin/env -S PYTHONPATH=/usr/lib/qcom-qairt/python /usr/bin/python3.12' "${pkgdir}/usr/bin/${python_tool}"
    done

    install -Dm644 "${_sdk_dir}/LICENSE.pdf" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.pdf"
    install -Dm644 "${_sdk_dir}/NOTICE.txt" "${pkgdir}/usr/share/licenses/${pkgname}/NOTICE.txt"
    install -Dm644 "${_sdk_dir}/QNN_NOTICE.txt" "${pkgdir}/usr/share/licenses/${pkgname}/QNN_NOTICE.txt"
}

package_qcom-qairt-sdk-v68() {
    pkgdesc='Qualcomm AI Runtime SDK with QNN, SNPE, QAIRT, and Genie (Hexagon v68)'
    depends=('gcc-libs' 'python312' 'python312-numpy' 'python312-pyyaml' 'python312-onnx' 'python312-pandas' 'python312-pydantic' 'python312-aenum')
    provides=('qcom-qairt-sdk' 'qcom-qnn-sdk' 'qcom-snpe-sdk' 'qcom-qnn-sdk-v68' 'qcom-snpe-sdk-v68')
    conflicts=('qcom-qairt-sdk-v73' 'qcom-qnn-sdk-v68' 'qcom-snpe-sdk-v68' 'qcom-qnn-sdk-v73' 'qcom-snpe-sdk-v73')
    replaces=('qcom-qnn-sdk-v68' 'qcom-snpe-sdk-v68')

    _package_qairt_sdk 'hexagon-v68'
}

package_qcom-qairt-sdk-v73() {
    pkgdesc='Qualcomm AI Runtime SDK with QNN, SNPE, QAIRT, and Genie (Hexagon v73)'
    depends=('gcc-libs' 'python312' 'python312-numpy' 'python312-pyyaml' 'python312-onnx' 'python312-pandas' 'python312-pydantic' 'python312-aenum')
    provides=('qcom-qairt-sdk' 'qcom-qnn-sdk' 'qcom-snpe-sdk' 'qcom-qnn-sdk-v73' 'qcom-snpe-sdk-v73')
    conflicts=('qcom-qairt-sdk-v68' 'qcom-qnn-sdk-v68' 'qcom-snpe-sdk-v68' 'qcom-qnn-sdk-v73' 'qcom-snpe-sdk-v73')
    replaces=('qcom-qnn-sdk-v73' 'qcom-snpe-sdk-v73')

    _package_qairt_sdk 'hexagon-v73'
}
