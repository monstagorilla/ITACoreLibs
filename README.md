## ITACoreLibs

ITACoreLibs are a collection of C++ libraries with basic tools for virtual acoustics including real-time audio streaming components for synthesis and reproduction.


### Purpose and license

The ITACoreLibs have been made available to the public in the spirit of sharing knowledge, giving possibility to build upon it and employing it for scientific research that should be reproducible. In practice this usually requires a binary version of ITACoreLibs components to be uploaded to an open access research repository for a certain amount of time (like 10 years).

The final license of the ITACoreLibs binary package depends on the *build configuration* and hence a *license compatibility with depending packages* that are linked against the components (in case of dynamic linking) and/or an application. Therefore, we have decided to generally use the permissive Apache License, Version 2.0, for our entire code base. It releases any researcher from the burden of imposing a dedicated license, yet requires stating the name and making clear where the original source of the version used can be obtained from - very much like a thorough (data) citation in a scientific publication.

Unfortunately, any copyleft license, notably the GNU General Public License (GPL), is incompatible using it this way (distributing Apache licensed binary packages that link against GPL libraries). ITACoreLibs is not per se using GPL dependencies (hence we do not per se use GPL for our code) but there are build configurations that demand it. As a solution, we license those ITACoreLibs binary distributions (linking against GPL libraries) under the respective license, subjogating to the viral nature of copyleft. This approach has similarity to dual licensing Apache and newer GPL, but we do not give a *choice* as the Apache license should be favored when no copyleft dependency is included, as it is compatible with newer GPL licenses anyway (in other words: *can be linked against by a GPL binary*). We clearly state that we support the idea of open source software and want to act in that spirit by encouraging further open source usage. However, we do not want to enforce a specific copyleft license to be used in open science practice, as this might exclude scientific institutions that can not follow this path.

#### ITACoreLibs code license

Copyright 2015-2020 Institute of Technical Acoustics (ITA), RWTH Aachen University.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use files of this project except in compliance with the License.
You may obtain a copy of the License at

<http://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

#### ITACoreLibs binaries license (selective license depending on third party library licenses)

If a distributed binary package of ITACoreLibs is provided for download and (partly) links against GPL libraries, the resulting binaries will be published under the GPL license. This occurs when a certain combination of dependencies created by the ITACoreLibs build configuration pulls in copyleft libraries.

If ITACoreLibs build configuration has made it necessary, find the corresponding GPL license file named `COPYING` in the root folder of a ITACoreLibs distribution. The Apache license formulating the license for the ITACoreLibs code will always be named LICENSE.md, and only refers to the source files as well as to ITA libraries that are _not linking against an GPL library_.

> A warning note: if you have received an ITACoreLibs package and are linking against an ITACoreLibs component library subject to GPL, be aware that you have to make your work compatible with GPL, if it is necessary (if there is a COPYING file).
> If you are redistributing an ITACoreLibs component, do not remove any license file.
> If you are insecure, there is no harm in licensing your source and binary under GPL if you are OK sharing your code and you don't believe that anyone will be building upon your work (and imposing to continue using GPL along the dependency chain).



### Quick build guide

Follow instructions from Wiki pages of [ITACoreLibs](https://git.rwth-aachen.de/ita/ITACoreLibs/wikis/home) project.


### Support disclaimer

This project has been made available for the public by the open science initiative of ITA.
The act aims at improving transparency and reproducibility of research outcomes that have been funded by public institutions, most prominently the DFG (German research foundation).
Usage of the shared content is highly appreciated, but ITA cannot give any support outside of established collaboration agreements.
