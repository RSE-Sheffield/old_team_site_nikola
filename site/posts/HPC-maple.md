<!--
.. title: High Performance Computing with Maple
.. author: Mike Croucher
.. slug: HPC-Maple
.. date: 2016-05-26 12:40:19 UTC+01:00
.. tags:
.. category:
.. link:
.. description:
.. type: text
-->

Many people who use Maple on Sheffield's High Performance Computing (HPC) cluster do so interactively. They connect to the system, start a graphical X-Windows session and use Maple in exactly the same way as they would use it on their laptop. Such usage does have some benefits, giving access to more CPU cores and memory than you'd get on even the most highly specified of laptops.

Interactive usage on the HPC system also has problems. Thanks to [network latency](https://en.wikipedia.org/wiki/Latency_(engineering)), using a Graphical User Interface over an X-Windows connection can be a painful experience. Additionally, long calculations can tie up your computer for hours or days and if anything happens to the network connection, you risk losing it all!

If you spend a long time waiting for your Maple calculations to run, it's probably time to think about moving to batch processing.

## Batch processing
