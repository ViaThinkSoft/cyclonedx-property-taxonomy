

CycloneDX Property Taxonomy by ViaThinkSoft
===========================================

This document defines the namespaces and properties used by the `vts` namespace in the [CycloneDX Property Taxonomy](https://github.com/CycloneDX/cyclonedx-property-taxonomy).

These optional properties are valid for components in a CycloneDX SBOM file and can be used by anyone, not just for ViaThinkSoft products:

- `vts:disposition`

  Defines how a component is represented in the SBOM and its relationship to the delivered product.

  Possible values are:

  - `artifact`
    The component exists as a standalone file delivered with the product, or as a standalone file after installing and/or unpacking the software.
    
    Examples:
    * EXE, DLL, DRV...
    * BPL
    * PDF
    * XML
    * Configuration files

  - `embedded`
    The component's content, or a compiled/processed representation of the component, is included in a delivered artifact and is not delivered, unpacked, or installed as a standalone file.
    
    Examples:
    * a font stored as a PE resource
    * a file contained in an archive
    * generated source or resource files (such as protobuf output), source code files, or collections of source code files (e.g. JEDI), whose compiled code and data are included in a PE binary
    * precompiled units (e.g. Delphi DCU) whose compiled code and data are included in a PE binary

  - `external`
    The component is not part of the product but is required or expected to be provided by the target environment.
    Since it is not shipped with the product, version information may not be determinable unless the product requires an exact version.
    
    Examples:
    * Visual C++ Redistributable
    * .NET Framework
    * ODBC Driver
    * Java Runtime Environment
    
    Note that if runtime binaries are shipped with the product, there needs to be an additional component with disposition `artifact`.

  - `absent`
    The component itself is not part of the product, but generated binary content originating from this component is included in a delivered artifact.
    
    Examples:
    * RTTI, Startup code, initialization/finalization code, exception tables, etc. that were created by a compiler.
      Note that the component should represent the generated contribution, not the tool that created it.
      For example, the component is "Delphi compiler-generated startup code", not "Delphi compiler".
      Also note that language-provided runtimes (such as the Delphi VCL/RTL) are better included as separate components with disposition `embedded`, since the VCL/RTL is mostly independent from the actual Pascal compiler.
    * .NET PackageReferences/NuGet packages that caused assembly references to be emitted into the resulting .NET assembly.
      The referenced runtime assembly (e.g., DLL) should be represented separately with disposition `artifact` if it is delivered with the product.

- `vts:install_location`

  Default location on the end-user's system where the artifact is stored.

  Examples
  * On Windows, the path is typically `C:\Program Files\...`
  * On Linux, applications may be installed in locations such as `/usr/bin/...`
  * On Mac OS X, the default location is usually `/Applications/...`
  * For web applications, `/var/www/...` can be used to indicate a directory managed by the web server.

- `vts:discovery`

  Describes the origin and method by which the component was discovered.
  
  Possible values:
    * `filesystem`
       The component was discovered by scanning the filesystem (e.g. installed files, libraries, binaries).
    *  `project`
       The component was discovered by analyzing a project definition, build configuration, or source tree.
    * `manual`
       The component was manually added or confirmed by a user.
       
- `vts:discovery:confidence`

  Describes the confidence level of the component discovery.
  
  Possible values:
    * `confirmed`
      The component was found, and there is strong evidence that it is used or belongs to the analyzed system.
    * `uncertain`
      The component was found, but its actual usage or relationship to the analyzed system could not be confirmed.
    * `inferred`
      The component was not directly identified; its presence or usage is only assumed based on indirect evidence.
      
- `vts:discovered_by`

  In case multiple tools were used to detect components, this property contains the name of the tool. It should fit the tool described in `metadata.tools[].bom-ref` (preferred), or `metadata.tools[].name`.
       
- `vts:encrypted`

  Describes whether the majority of the file is encrypted or not.
  
  Values: `yes`, `no`, `unknown`
  
- `vts:obfuscated`

  Describes whether the majority of the file is obfuscated or not.

  Values: `yes`, `no`, `unknown`
  
- `vts:compressed`

  Describes whether the majority of the file is compressed, such as a JPEG file, a ZIP archive, an OpenDocument document, or an UPX packed executable

  Values: `yes`, `no`, `unknown`

- `vts:used_toolchain`

  Toolchain information, such as primary programming language, compiler and version, linker and version, resource compiler and version, etc.

  Example: `Delphi Compiler 12.0; Borland Resource Compiler 5.82; Delphi Linker 12.0`

- `vts:debuginfo`

  Describes the availability and location of debug information.

  Possible values:
    * `included`
      Debug information is embedded in the binary artifact itself.
    * `external`
      Debug information exists as a separate artifact delivered together
      with the product (e.g. PDB file).
    * `absent`
      No debug information is available or delivered.
    * `unknown`
      The presence or absence of debug information could not be determined.

- `vts:security:...`

  Various security features of the binary file. The list is most likely not complete; hence please let us know if you have more suggestions (create a [GitHub issue](https://github.com/ViaThinkSoft/cyclonedx-property-taxonomy/issues)).

- `vts:security:dep_nx`

  Supports Data Execution Prevention / NX bit.

  Values: `disabled`, `enabled`, `unknown`

- `vts:security:aslr`

  Supports Address Space Layout Randomization.

  Values: `disabled`, `enabled`, `unknown`

- `vts:security:high_entropy_aslr`

  Supports the Extension for ASLR for 64-bit processes.

  Values: `disabled`, `enabled`, `unknown`

- `vts:security:cfg`
  
  Supports Control Flow Guard. PE Load Config Directory (GuardFlags)

  Values: `disabled`, `enabled`, `unknown`
  
- `vts:security:safe_seh`

  Supports Safe Structured Exception Handling. PE-Feature for 32-Bit x86

  Values: `disabled`, `enabled`, `unknown`

- `vts:security:stack_cookie`
  
  Supports Stack Canary / `/GS` Compiler protection (MSVC)

  Values: `disabled`, `enabled`, `unknown`

- `vts:security:code_signing`

  The binary file (EXE, PDF, etc.) was signed.

  Values: `disabled`, `enabled`, `unknown`

- `vts:security:code_signing:digest_algo`

  Digest algorithm used for the code signature, e.g. `sha256`

- `vts:security:code_signing:root_ca`

  Root CA that signed the certificate

  Example: `GlobalSign Code Signing Root R45`

- `vts:security:code_signing:root_ca:fingerprint`
  
  Fingerprint of the Root CA certificate

  Example `4efc31460c619ecae59c1bce2c008036d94c84b8`

- `vts:3p:...`
  
  Anything third-party (3p = 3rd party) related.

- `vts:3p:virustotal:url`

  URL to the analysis page of the binary file.
  If this property is used, the file MUST have been uploaded to VirusTotal (i.e. not just contain the predicted URL according to the file hash).

- `vts:3p:virustotal:scanresult`

  Timestamp (format: ISO 8601 / RFC 3339), latest result (`detected || "/" || total`), and community score, at the time of the SBOM generation, delimited by whitespaces.

  Example: `2026-07-31T10:44:00Z 5/71 32` means that the latest scan (at the time of the SBOM generation) was July 31st 2026, 10:44 GMT, 5 out of 71 vendors detected the file as malicious, and it had a community score of +32.

- `vts:3p:msft:dotnet:asm:...` assemblyIdentity in the style of .net assembly files.

- `vts:3p:msft:dotnet:asm:assemblyName`, e.g. `MyLibrary`

- `vts:3p:msft:dotnet:asm:version`, e.g. `1.0.0.0`

- `vts:3p:msft:dotnet:asm:culture`, e.g. `de-DE`

- `vts:3p:msft:dotnet:asm:publicKeyToken`, e.g. `6595b64144ccf1df`

- `vts:3p:msft:dotnet:asm:processorArchitecture`, e.g. `MSIL`

- `vts:3p:msft:pe:asm:...`
  assemblyIdentity in the style of Manifest files, such as COM libraries.

- `vts:3p:msft:pe:asm:type`, e.g. `win32`

- `vts:3p:msft:pe:asm:name`, e.g. `MyApp`

- `vts:3p:msft:pe:asm:version`, e.g. `1.0.0.0`

- `vts:3p:msft:pe:asm:processorArchitecture`, e.g. `amd64`

- `vts:3p:msft:pe:asm:language`, e.g. `de-DE`

- `vts:3p:msft:pe:versioninfo:...` Version information data as [described by Microsoft](https://learn.microsoft.com/en-us/windows/win32/menurc/versioninfo-resource).

- `vts:3p:msft:pe:versioninfo:fileflags`, e.g. `VS_FF_PATCHED | VS_FF_PRERELEASE`

- `vts:3p:msft:pe:versioninfo:fileos`, e.g. `VOS__WINDOWS32`

- `vts:3p:msft:pe:versioninfo:filetype`, e.g. `VFT_DRV`

- `vts:3p:msft:pe:versioninfo:subtype`, e.g. `VFT2_DRV_KEYBOARD`

- `vts:3p:msft:pe:versioninfo:langId`, e.g. `0x0407` (German)

- `vts:3p:msft:pe:versioninfo:charsetId`, e.g. `1252` (Multilingual)

- `vts:3p:msft:pe:versioninfo:comments` as described by Microsoft.

- `vts:3p:msft:pe:versioninfo:companyName` as described by Microsoft.

- `vts:3p:msft:pe:versioninfo:fileDescription` as described by Microsoft.

- `vts:3p:msft:pe:versioninfo:fileVersion` as described by Microsoft.

- `vts:3p:msft:pe:versioninfo:internalName` as described by Microsoft.

- `vts:3p:msft:pe:versioninfo:legalCopyright` as described by Microsoft.

- `vts:3p:msft:pe:versioninfo:legalTrademarks` as described by Microsoft.

- `vts:3p:msft:pe:versioninfo:originalFilename` as described by Microsoft.

- `vts:3p:msft:pe:versioninfo:privateBuild` as described by Microsoft.

- `vts:3p:msft:pe:versioninfo:productName` as described by Microsoft.

- `vts:3p:msft:pe:versioninfo:productVersion` as described by Microsoft.

- `vts:3p:msft:pe:versioninfo:specialBuild` as described by Microsoft.


Suggestions
-----------

If you have ideas for more properties, please let us know by creating a [GitHub issue](https://github.com/ViaThinkSoft/cyclonedx-property-taxonomy/issues)!


License
-------

Copyright (C) 2026 Daniel Marschall, ViaThinkSoft

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
