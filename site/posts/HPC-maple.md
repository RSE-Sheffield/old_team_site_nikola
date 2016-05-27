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

Many people who use [Maple](http://www.maplesoft.com/) on Sheffield's High Performance Computing (HPC) cluster do so interactively. They connect to the system, start a graphical X-Windows session and use Maple in exactly the same way as they would use it on their laptop. Such usage does have some benefits: giving access to more CPU cores and memory than you'd get on even the most highly specified of laptops.

Interactive usage on the HPC system also has problems. Thanks to [network latency](https://en.wikipedia.org/wiki/Latency_(engineering)), using a Graphical User Interface over an X-Windows connection can be a painful experience. Additionally, long calculations can tie up your computer for hours or days and if anything happens to the network connection during that time, you risk losing it all!

If you spend a long time waiting for your Maple calculations to run, it's probably time to think about moving to batch processing.

## Batch processing

The idea behind batch processing is that you log in to the system, send your computation to a queue and then log out and get on with your life. The HPC system will process your computation when resources become available and email you when it's done. You can then log back in, transfer the results to your laptop and continue your analysis.

So, batch processing frees up your personal computer but it can also significantly increase your throughput. With batch processing, you can submit hundreds of computations to the queue simultaneously.  The system will automatically process as many of them as it can in parallel -- allowing you to make use of dozens of large computers at once.

Batch processing is powerful but it comes at a price and that price is complexity.

## Converting interactive worksheets to Maple language files

You are probably used to interacting with Maple via richly formatted worksheets and documents. These have the file extension **.mw**. Unfortunately, it is not possible to run Maple worksheets in batch mode so it is necessary for us to convert them to **maple language files** instead.

A [Maple Language File](http://www.maplesoft.com/support/help/maple/view.aspx?path=Formats%2FMPL) has the extension **.mpl** and is a pure text file. To convert a worksheet to a Maple Language File, open the worksheet and click on  **File->Export As->Maple Input**

![Convert to mpl](/images/convert_to_mpl.png)
