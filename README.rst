
=============================
Release Python Package Action
=============================

This is a composite GitHub action to build and python package and upload it to PyPI.
To run this you will need to allow your action to publish packages to PyPI through the `trusted publishers <https://docs.pypi.org/trusted-publishers/>`_ framework.
Also, you will need to set the following permissions::

  permissions:
    contents: write
    id-token: write

This action has the following inputs::

  inputs:
    pypi-package-name:
      description: 'Specify the name of the python package on PyPI.org.'
      required: true
      default: 'not-a-name'
    release-heading:
      description: 'Specify the heading for the release.'
      required: false
      default: '${{ github.ref }}'

Example action file for your repository::

  name: Deploy library

  on:
    push:
      tags:
        - "v*.*.*"

  jobs:
    build-and-release-package:
      if: github.repository == 'cmlibs-python/cmlibs.maths'
      runs-on: ubuntu-20.04
      name: Release package
      permissions:
        contents: write
        id-token: write  # IMPORTANT: this permission is mandatory for trusted publishing
      steps:
        - name: Release Python package
          uses: hsorby/release-python-package-action@v1
          with:
            pypi-package-name: cmlibs.maths
