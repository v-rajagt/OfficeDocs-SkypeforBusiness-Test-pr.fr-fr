﻿---
title: Clear-CsDeviceUpdateFile
TOCTitle: Clear-CsDeviceUpdateFile
ms:assetid: 34c5bb61-fcba-4e93-bb21-83b9611f3045
ms:mtpsurl: https://technet.microsoft.com/fr-fr/library/Gg425835(v=OCS.15)
ms:contentKeyID: 49296835
ms.date: 05/20/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Clear-CsDeviceUpdateFile

 

_**Dernière rubrique modifiée :** 2015-03-09_

Deletes any rejected device update files that are no longer associated with a device. Cette applet de commande est une nouveauté de Lync Server 2010.

## Syntaxe

    Clear-CsDeviceUpdateFile -Identity <XdsIdentity> [-Confirm [<SwitchParameter>]] [-Force <SwitchParameter>] [-WhatIf [<SwitchParameter>]]

## Examples

## EXAMPLE 1

The command shown in Example 1 deletes all the device update files from the service WebServer:atl-cs-001.litwareinc.com that are no longer associated with a device.

    Clear-CsDeviceUpdateFile -Identity "service:WebServer:atl-cs-001.litwareinc.com"

## Detailed Description

Each time new device updates are uploaded to the system, a corresponding device update rule is created. By default, these new device update rules are assigned to the Pending state; that means that the rules can be downloaded and installed on test devices, but not on production devices. In turn, this gives you an opportunity to test the updates before making them available to users. If testing is successful, you can then run the **Approve-CsDeviceUpdateRule** cmdlet to make these device updates available to users.

If testing is not successful then you can use the **Reset-CsDeviceUpdateRule** cmdlet or the **Restore-CsDeviceUpdateRule** cmdlet to reject an update. When these cmdlets are run, the device update is disassociated from its device update rule. At that point, administrators can then use the **Clear-CsDeviceUpdateFile** cmdlet to remove the disassociated updates from the server.

Who can run this cmdlet: By default, members of the following groups are authorized to run the **Clear-CsDeviceUpdateFile** cmdlet locally: RTCUniversalServerAdmins. To return a list of all the role-based access control (RBAC) roles this cmdlet has been assigned to (including any custom RBAC roles you have created yourself), run the following command from the Windows PowerShell prompt:

Get-CsAdminRole | Where-Object {$\_.Cmdlets –match "Clear-CsDeviceUpdateFile"}

## Paramètres


<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Required</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>Identity</em></p></td>
<td><p>Required</p></td>
<td><p>Microsoft.Rtc.Management.Xds.XdsIdentity</p></td>
<td><p>Unique identifier of the service hosting the device update files. For example, this syntax clears device update files from the services web service for the pool atl-cs-001.litwareinc.com: -Identity &quot;service:WebServer:atl-cs-001.litwareinc.com&quot;.</p></td>
</tr>
<tr class="even">
<td><p><em>Confirm</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Vous demande confirmation avant d’exécuter la commande.</p></td>
</tr>
<tr class="odd">
<td><p><em>Force</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Suppresses the display of any non-fatal error message that might occur when running the command.</p></td>
</tr>
<tr class="even">
<td><p><em>WhatIf</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Décrit ce qui se passe si vous exécutez la commande sans l’exécuter réellement.</p></td>
</tr>
</tbody>
</table>


## Input Types

None. The **Clear-CsDeviceUpdateFile** cmdlet does not accept pipelined input.

## Return Types

None. The **Clear-CsDeviceUpdateFile** cmdlet does not return any values.

## Voir aussi

#### Autres ressources

[Clear-CsDeviceUpdateLog](clear-csdeviceupdatelog.md)  
[Get-CsDeviceUpdateConfiguration](get-csdeviceupdateconfiguration.md)

