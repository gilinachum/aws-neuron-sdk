.. _mxnet-neuron-main:
.. _neuron-mxnet:


MXNet Neuron
============
MXNet Neuron unlocks high-performance and cost-effective deep learning acceleration on AWS Trainium-based and Inferentia-based Amazon EC2 instances.

MXNet Neuron enables native MXNet models to be accelerated on Neuron devices, so you can use your existing framework application and get started easily with minimal code changes. 
 


.. tab-set::


   .. tab-item:: Inference
        :name: torch-neuronx-training-main

        .. dropdown::  Setup Guide for Inf1 
                :class-title: sphinx-design-class-title-med
                :class-body: sphinx-design-class-body-small
                :animate: fade-in
                

                .. toctree::
                    :maxdepth: 1

                    Fresh install </frameworks/mxnet-neuron/setup/mxnet-install>
                    Update to latest release </frameworks/mxnet-neuron/setup/mxnet-update>
                    Install previous releases </frameworks/mxnet-neuron/setup/mxnet-install-prev>


        .. dropdown::  Tutorials
                :class-title: sphinx-design-class-title-med
                :animate: fade-in
                

                .. tab-set::

                    .. tab-item:: Computer Vision Tutorials
                                :name:   

                                * ResNet-50 tutorial :ref:`[html] </src/examples/mxnet/resnet50/resnet50.ipynb>` :mxnet-neuron-src:`[notebook] <resnet50/resnet50.ipynb>`
                                * Model Serving tutorial :ref:`[html] <mxnet-neuron-model-serving>`
                                * Getting started with Gluon tutorial :ref:`[html] </src/examples/mxnet/mxnet-gluon-tutorial.ipynb>` :github:`[notebook] </src/examples/mxnet/mxnet-gluon-tutorial.ipynb>`


                                .. toctree::
                                        :hidden:

                                        /src/examples/mxnet/resnet50/resnet50.ipynb
                                        /frameworks/mxnet-neuron/tutorials/tutorial-model-serving
                                        /src/examples/mxnet/mxnet-gluon-tutorial.ipynb

                    .. tab-item:: Natural Language Processing (NLP) Tutorials
                                :name:

                                * MXNet 1.8: Using data parallel mode tutorial :ref:`[html] </src/examples/mxnet/data_parallel/data_parallel_tutorial.ipynb>` :mxnet-neuron-src:`[notebook] <data_parallel/data_parallel_tutorial.ipynb>`

                                .. toctree::
                                        :hidden:

                                        /src/examples/mxnet/data_parallel/data_parallel_tutorial.ipynb

                    .. tab-item:: Utilizing Neuron Capabilities Tutorials
                                :name:

                                * NeuronCore Groups tutorial :ref:`[html] </src/examples/mxnet/resnet50_neuroncore_groups.ipynb>` :mxnet-neuron-src:`[notebook] <resnet50_neuroncore_groups.ipynb>`


                                .. toctree::
                                        :hidden:

                                        /src/examples/mxnet/resnet50_neuroncore_groups.ipynb


                .. note::

                        To use Jupyter Notebook see:

                        * :ref:`setup-jupyter-notebook-steps-troubleshooting`
                        * :ref:`running-jupyter-notebook-as-script`                                         

        .. dropdown::  API Reference Guide
                :class-title: sphinx-design-class-title-med
                :class-body: sphinx-design-class-body-small
                :animate: fade-in

                .. toctree::
                    :maxdepth: 1

                    /frameworks/mxnet-neuron/api-compilation-python-api

        .. dropdown::  Developer Guide
                :class-title: sphinx-design-class-title-med
                :class-body: sphinx-design-class-body-small
                :animate: fade-in
                

                .. toctree::
                    :maxdepth: 1

                    /general/appnotes/mxnet-neuron/flex-eg

        .. dropdown::  
                :class-title: sphinx-design-class-title-med
                :class-body: sphinx-design-class-body-small
                :animate: fade-in
                :open:
                

                .. toctree::
                    :maxdepth: 1

                    /frameworks/mxnet-neuron/troubleshooting-guide
                    What's New </release-notes/mxnet-neuron/mxnet-neuron>
                    /release-notes/compiler/neuron-cc/neuron-cc-ops/neuron-cc-ops-mxnet
