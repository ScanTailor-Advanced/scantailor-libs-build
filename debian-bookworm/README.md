Here are the steps for building and installing ScanTailor Advanced on Debian bookworm:

```sh
# Install dependencies for these steps
apt install equivs wget

# Install ScanTailor build dependencies
wget https://github.com/ScanTailor-Advanced/scantailor-libs-build/blob/master/debian-bookworm/builddep-scantailor.ctl
equivs-build builddep-scantailor.ctl
sudo dpkg --install builddep-scantailor_1.0_all.deb
sudo apt install # To satisfy the dependencies

# Build
cd scantailor-advanced/ # This is the git source
mkdir build
cd build
cmake ..
cmake --build .

# Create package and install it
cpack -G "DEB"
sudo dpkg --install scantailor-advanced-*-Linux.deb

# Remove build dependencies
sudo apt purge builddep-scantailor
```
