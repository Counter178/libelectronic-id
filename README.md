# libelectronic-id

![European Regional Development Fund](https://github.com/e-gov/RIHA-Frontend/raw/master/logo/EU/EU.png)

C++ library for performing cryptographic operations with electronic identification (eID) cards.

Currently supports Finnish, Estonian, Latvian and Lithuanian eID cards. Please
submit an issue if you want to request support for your country's eID card.

If possible, communicates with the eID card directly via PC/SC using APDUs
according to the card specification.

When APDU communication is not possible (e.g. Lithuanian eID), uses PKCS#11 and
requires the corresponding PKCS#11 module to be installed.

## Usage

Example how to automatically select and connect to a supported eID card, and
read the authentication certificate:

    const auto cardInfo = autoSelectSupportedCard();
    std::cout << "Reader " << cardInfo->reader().name << " has supported card "
                    << cardInfo->eid().name();

    const auto certificateBytes = cardInfo->eid().getCertificate(CertificateType::AUTHENTICATION);

See more examples in [tests](tests).

## Building

    apt install build-essential pkg-config cmake libgtest-dev valgrind libpcsclite-dev
    sudo bash -c 'cd /usr/src/googletest && cmake . && cmake --build . --target install'

    cd build
    cmake .. # optionally with -DCMAKE_BUILD_TYPE=Debug
    cmake --build . # optionally with VERBOSE=1

## Testing

Build as described above, then run inside `build` directory:

    ctest # or 'valgrind --leak-check=full ctest'

`ctest` runs tests that use the _libscard-mock_ library to mock PC/SC API calls.

There are also integration tests that use the real operating system PC/SC
service, run them inside `build` directory with:

    ./libpcsc-cpp-test-integration

## Development guidelines

- Format code with `scripts/clang-format.sh` before committing
- See [docs/DEVELOPMENT.md](docs/DEVELOPMENT.md)
