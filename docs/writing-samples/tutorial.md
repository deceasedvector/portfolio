---
title: Tutorial
---

# Placing an Associate in a Tree 

???+ example "Meta"

    * **Tool**: Readme.io in Markdown
    * **Original**: [https://developers.directscale.com/v1.0/docs/placing-an-associate-in-a-tree](https://developers.directscale.com/v1.0/docs/placing-an-associate-in-a-tree)
    * **About this sample**: While building out DirectScale's Developer documentation site, I needed tangible examples of real code that a developer could start from. Looking through the examples our SDET developer prepared, I came across this one.

        With the code ready and tested, my goal was to break down the important elements and, in its original form, link out to the applicable API Reference docs. This particular example is for a very specific audience of developers integrating with DirectScale's Client Extension to extend the default Commission Plan template with .NET. 

## Introduction

There are times when you may need to place an individual in the Tree using custom code. The Client Extension is a great tool to aid in this endeavor, but the method and syntax aren't always obvious. With this resource, we'll break down an example of placing an Associate in a Tree structure.

!!! warning "Important"
    Trees are important. Making mistakes can be challenging to fix. Please make sure you're comfortable with Tree structure, Tree types and that you know how to test and check the results of moves in Corporate Admin.

### Before we start

It's essential to understand the concept of a "Node". You'll see this term referenced throughout this resource and others that reference Tree movements/placement. 

Essentially, a Node is a spot in the Tree with information about itself, such as who the parent Node is and who it contains. It's common to refer to a Node as an Associate because, for most, that's what it is.

## Full Example

Take a look at the full example before we get started.

=== "C#"

    ```csharp
    var _nodeId = new DirectScale.Disco.Extension.NodeId(_associate.AssociateId);
    var enrollernodeDetail = _treeService.GetNodeDetail(_nodeId, TreeType.Enrollment);

    try
    {
        _treeService.ValidatePlacements(
            new Placement[] {
                new Placement() { 
                    Tree = TreeType.Unilevel, 
                    NodeDetail = new NodeDetail() { NodeId = _nodeId, UplineId = enrollernodeDetail.UplineId, UplineLeg = LegName.Empty } 
                }
            });
        _treeService.Place(new Placement[] { 
            new Placement() { 
                Tree = TreeType.Unilevel, 
                NodeDetail = new NodeDetail() { NodeId = _nodeId, UplineId = enrollernodeDetail.UplineId, UplineLeg = LegName.Empty } 
            }; 
        });
    }
    ```

Let's break this example down.

## 1. Declare a variable for `nodeId`

=== "C#"

    ```csharp
    var _nodeId = new DirectScale.Disco.Extension.NodeId(_associate.AssociateId);
    ```

We set the variable equal to the `nodeId` class, which indicates a Tree placement with the property set to `AssociateId`. There's another property you can use as well called `TreeIndex`. 

Most of the time, the property `TreeIndex` will be `0`, and only the `AssociateId` will be used unless you allow Associates to be placed in the same Tree multiple times (like in Matrix Trees).

## 2. Declare a variable for the Node details

Declare a variable that calls the `ITreeService` `GetNodeDetail(NodeId, TreeType)` method to get the upline and Leg details for the Associate (Node).

=== "C#"

    ```csharp
    var enrollernodeDetail = _treeService.GetNodeDetail(_nodeId, TreeType.Enrollment);
    ```

The parameters are:

Parameter  | Definition
-----------|------------
`NodeId`   | Set to the Associate to find upline.
`TreeType` | Set to the in which to find upline.

For this example, we're placing in the Enrollment Tree (`TreeType.Enrollment`).

## 3. Validate the Associate's placement

Use the `ValidatePlacements(Placement[])` method to validate the changes before the placements are executed. This method is vital to call first to make sure you don't create circular references.

=== "C#"

    ```csharp
    _treeService.ValidatePlacements(
        new Placement[] {
            new Placement() { 
                Tree = TreeType.Unilevel, 
                NodeDetail = new NodeDetail() { NodeId = _nodeId, UplineId = enrollernodeDetail.UplineId, UplineLeg = LegName.Empty } 
            }
    });
    ```

For Unilevel and Enrollment Trees, you must specify the `LegName` as `LegName.Empty` and not `null` or you'll get null reference exception errors. For Binary or Matrix Trees, you can use the appropriate `LegNames` (`LegName.Left`, `LegName.Middle`, `LegName.Right`, and so on).

## 4. Execute the placement

After you've verified the placement with `ValidatePlacements()` in the preceding step, use the `Place([])` method to executes one or more placements or moves:

=== "C#"

    ```csharp
     _treeService.Place(new Placement[] { 
        new Placement() { 
            Tree = TreeType.Unilevel, 
            NodeDetail = new NodeDetail() { NodeId = _nodeId, UplineId = enrollernodeDetail.UplineId, UplineLeg = LegName.Empty } 
            }; 
    });
    ```