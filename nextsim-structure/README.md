_2025 SASIP Workshop_

Demonstration session:

## neXtSIM: Structure and Design Principles
Tim Spain and Einar Ólason (NERSC, Bergen, Norway)

### What we will do in this demonstration session:

We will introduce the structure and design principles of the latest neXtSIM version. The model is written using ISO C++17, and the design aims to take advantage of the language's object orientation. This README lists the most important basic concepts. The session itself will delve into this in more detail.

### Simplified program call sequence

The call structure is based on creating and calling four main objects: ``Model``, ``Iterator``, ``Iterant``, and ``PrognosticData``. ``PrognosticData``, in turn, calls four main modules derived from the interface classes ``IAtmosphereBoundary``, ``IOceanBoundary``, ``IDynamics``, and ``IceGrowth`` (of which there is still only one instance). The simplified program call sequence (ignoring several initialisation and I/O calls) looks like this:

```
main.cpp
    |-- Model model
        |-- PrognosticData pData
        |-- Iterator iterator
        |-- Iterant modelStep
        |-- model.configure()
            |-- pData.configure()
                |-- tryConfigure(IAtmBdyDerived)
                |-- tryConfigure(IOcnBdyDerived)
                |-- tryConfigure(IDynamicsDerived)
                |-- tryconfigure(iceGrowth)
        |-- model.run()
            |-- iterator.run()
                |-- (main model loop)
                    |-- modelStep.iterate()
                        |-- pData.update()
                            |-- IOceanBndyDerived.updateBefore()
                            |-- IAtmBndyDerived.update()
                            |-- iceGrowth.update()
                            |-- IDynamicsDerived.update()
                            |-- IOceanBndyDerived.updateAfter()
```

Each module contains a core functionality, which can be replaced through the module system (see below). This allows developers to easily implement new boundary conditions via the ``IAtmosphereBoundary`` or ``IOceanBoundary`` base classes, new sea-ice dynamics via the ``IDynamics`` base class, and new thermodynamics via a future ``IIceGrowth`` base class. Each module can then contain sub-modules, which contain specific functionality, such as an albedo parameterisation, for example.


### Modules

#### Rationale

One of the key features of nextSIM-DG is the code's run-time modularity. This allows a user to change the model's parameterisations without recompiling. Instead, many parameterisations and model components can be changed by changing the configuration in the config file or by providing overrides on the command line. The nextSIM-DG module system is designed to do this with a minimal impact on the model's performance.

#### Usage

The user must provide additional configuration values to the model to use the module system to change the model configuration from its defaults. These can be permanently edited into the initial config file or provided as an override on the command line. The modules section of the config file starts with the section title ``[Modules]``. Below this are key-value pairs, one per line with the format ``key = value``. The key is the name of the directory that contains the module. This will be a subdirectory of one of the ``modules`` subdirectories in the nextSIM-DG code tree.

An example of this is the subdirectory ``core/src/modules/DiagnosticOutputModule`` which would be in the config file as the key ``DiagnosticOutputModule =``. The value following the equals sign is then one of the module implementations. The names of these can be obtained from the online help system accessible using the command line argument ``--config-help``and the relevant file in this documentation directory.


### Model data storage

Most of the data the model uses for simulating sea ice is stored in objects of the type ``ModelArray``. This class provides n-dimensional array indexing and an interface to the DG components, as well as the element-mean values that are used by the column physics calculations. The class provides a limited number of types, enumerated by the enum class ``ModelArray::Type``, each related to a set number of axis dimensions, enumerated by ``ModelArray::Dimension``. This mechanism ensures that arrays over the same domain have the same extent, including arrays with and without DG components. The dimensions and types are set at compile time, but the dimensions' lengths can be set at run time.

To avoid sharing data indiscriminately, ``ModelArray`` data can be shared by ``ModelArrayRef`` objects, which store a pointer to the ``ModelArray`` and provide pass-through functions for indexing and the four basic mathematical operations. The ``ModelArrayRef`` class is designed mainly for the column physics part of the model, and so only provides indexing and arithmetic on the first, index 0 component of the referenced ``ModelArray``. The one exception is the function ``allComponents()``, which returns a reference to the underlying data type (currently ``Eigen::Array``), which includes all available components. This allows the arrays with full DG components to be passed to the ice dynamics sub-model.

Objects of the ``ModelArrayRef`` class interact with a ``ModelArrayReferenceStore``, which allows registration and retrieval of the pointers by name. This class also allows write access restriction to an object. A ``ModelArray`` must be registered by name to the ``ModelArrayReferenceStore``, and optionally can be declared RO (read-only) or read-write. The default is RO (read-only) if this argument is not provided. A ``ModelArrayRef`` has the name of the array it targets and an access specification as template parameters (for example, ``"hice"``, ice thickness, accessed as ``RO``, read-only). A run-time error occurs if a read-write ``ModelArrayRef`` attempts to point to the name of an array that has only been registered as read-only.

Four ``ModelArrayReferenceStore``s currently exist in the model. Two exist to share the data collected from reading the external coupling data sources, which copy data to the relevant entry of the ``ModelArrayReferenceStore``s described below, and will not be discussed further. The main model uses two``ModelArrayReferenceStore``s, the first contains most of the data shared around the column physics model, allowing the array containing the sea surface latent heat flux, from the flux calculation routines, to be used in the thermodynamics implementation. All of the ``ModelArray``s in this store are element-mean values and have no higher DG components. The second is the store of data to be advected. In the first instance, this includes values such as the ice thickness and concentration and any arrays from any module that need to be advected along with the ice. This store of advected fields can then provide ``ModelArrayRef`` s to components of the column physics model, which only allow access to the element-mean value of that field. Through the ``allComponents()`` member function of ``ModelArrayRef``, the same store can also provide access to the full array, including all DG components, to the ice dynamics implementation. This reference obtained is to the underlying ``DataType`` of ``ModelArray``, which can be passed to the dynamics and ``reinterpret_cast`` ed to a ``dgVector`` for direct use in the dynamics, without any copying of data.
