## Creating your own drop-in QM sub-package

We recommend using the existing drop-in files as a guide and adapting them to your specific needs. However, here are the step-by-step instructions:

1) Create a drop-in file in the directory: `etc/qm/containers/containers.conf.d`
2) Add it as a sub-package to `rpm/qm.spec`
3) Test it by running: `make clean && VERSION=YOURVERSIONHERE make rpm`
4) Additionally, test it with and without enabling the sub-package using (by default it should be disabled but there are cases where it will be enabled by default if QM community decide):

Example changing the spec and triggering the build via make (feel free to automate via sed, awk etc):

```bash
# Define the feature flag: 1 to enable, 0 to disable
# By default it's disabled: 0
%define enable_qm_dropin_img_tempdir 1

make clean && VERSION=YOURVERSIONHERE make rpm
```