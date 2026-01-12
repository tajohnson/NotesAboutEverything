find . -name '*.erb' -exec sh -c 'mv "$0" "${0%.erb}.j2"' {} \;
