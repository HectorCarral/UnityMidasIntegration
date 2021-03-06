# UnityMidasIntegration
Asset for performing requests to MIDAS using Unity.

This Unity package includes a few custom components for integration with [MIDAS](https://github.com/bwrc/midas), as well as some dependencies.

Dependencies included:

- [LitJson](https://github.com/lbv/litjson): for deserialization of the JSON received by MIDAS.

MIDAS integration:

The main part of this package is a folder that contains two subdirectories: Prefabs and Scripts. The integration is based on four components:

- MidasAddress: Simple class for holding the address of the MIDAS dispatcher. It consists of an IP address and a port. These are specified in the inspector and are formatted automatically by the class. The default address is 'http://127.0.0.1/8080'. Depending on the system architecture one ore more instances of this class might be needed: one for each MIDAS dispatcher. In general, only one MIDAS dispatcher is used, so only one MidasAddress is necessary.
- MidasRequest: Class for holding a request to be performed to MIDAS. Each request has two fundamental components: the name of the node to which the request is targeted, and the type of message (usually "metric"). It can have (and usually has) several other parameters: type of metric request, channels to be used for the request, time window and additional arguments. This class receives all of these parameters in the inspector. It automatically formats the request using all of the parameters specified. The default values are for an example request for heart rate to an ECG node. One instance of this class is required for each different request to be performed.
- MidasClient: Class that performs the request to MIDAS using a MidasAddress and a MidasRequest using the method SendRequest. SendRequest has to be called from a listener, providing both necessary arguments. It returns the value returned by MIDAS, which most of the time will be a double. In case that the expected return value is not a double, another version of this function is provided, which returns a raw JsonData that will have to be manually casted to its natural data type (e.g., string). Only one instance of this class is needed.
- MidasListener: This class is provided as the basis for a listener. Every listener has to inherit from this class. This class needs references to instances of the three previously explained classes: MidasClient, MidasAddress and MidasRequest. If desired (and by default) with the option 'repeat', it performs a request every few seconds, specified using the variable repeatFrequencySeconds. Alternatively, the function GetGenericData can be used from a child Listener to get data on demand at any specific moment. It uses the provided client to send the provided request to the provided address. It holds the result in a variable called 'data'.
- GenericListener: Generic class that inherits from MidasListener, provided as the basis for each listener. The name of this class and its variables and functions should be changed to more descriptive names (e.g. a class ArousalListener with a function GetArousal that gets the arousal).

Two prefabs are also provided, to be used optionally:

- MidasClient: object that holds a MidasClient and a MidasAddress. This assumes that only one MidasAddress is used. Otherwise, different instances of the class MidasAddress can be added as required in this object or in others.
- GenericListener: object that holds a GenericListener and a MidasRequest. This will have to be adapted for each desired listener.
