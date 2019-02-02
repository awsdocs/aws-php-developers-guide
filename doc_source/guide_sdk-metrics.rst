.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#################################
|SDKM| in the |sdk-php| Version 3
#################################

|SDKMlong| (|SDKM|\) enables enterprise customers to collect metrics from AWS SDKs on their hosts and clients shared with 
AWS Enterprise Support. SDK Metrics provides information that helps speed up detection and diagnosis of issues occurring in connections 
to AWS services for AWS Enterprise Support customers. 

As telemetry is collected on each host, it is relayed via UDP to localhost, where the |CW| agent aggregates the data and sends it 
to the |SDKM| service. Therefore, to receive metrics, you must add the |CW| agent to your instance.

Learn more about 
:CW-dg:`SDK Metrics <CloudWatch-Agent-SDK-Metrics>` in the |CW-dg|.

Each of the following topics describes how to set up, configure, and manage |SDKM| for the |sdk-php|.

.. toctree::
    :maxdepth: 1

    Authorize SDK Metrics <guide_sdk-metrics-set-permissions.rst>
    Set Up SDK Metrics  <guide_sdk-metrics-configure.rst>
    SDK Metric Definitions <guide_sdk-metrics-definitions.rst>