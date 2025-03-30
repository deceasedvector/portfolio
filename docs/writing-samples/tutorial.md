---
title: Code tutorial
---

# Placing an associate in a tree

Learn how to place Associates in specific positions within DirectScale's tree structures using the Client Extension.
Tree placement determines organizational relationships in multi-level marketing systems and affects commissions, rank qualifications, and reporting.

!!! warning
    Tree structures are critical to your business operations. Incorrect placements can be difficult to reverse and may impact commissions and genealogy. 
    Always test thoroughly in a non-production environment before implementing in production.

## Before you begin

Understand these key concepts:

* **Node**: a position in a tree structure containing an associate, with information about its relationship to other nodes.

* **TreeType**: DirectScale supports many tree structures:

  * **Enrollment Tree**: tracks sponsor relationships.
  * **Unilevel Tree**: genealogy where each associate can have unlimited first-level downlines.
  * **Binary Tree**: structure with a max of two direct downlines per node.
  * **Matrix Tree**: structure with fixed width and unlimited depth. For example, 3×3, 4×7.

* **LegName**: designates a node's position relative to its upline (important for tree types with named positions like Binary's Left/Right).

## Initialize dependencies

Inject the necessary services into your class:

```csharp
private readonly ITreeService _treeService;
private readonly IAssociateService _associateService;

public YourClass(ITreeService treeService, IAssociateService associateService)
{
    _treeService = treeService;
    _associateService = associateService;
}
```

## Create a nodeId

Reference the associate you want to place:

```csharp
// For a standard placement
int associateId = 12345; 
var nodeId = new DirectScale.Disco.Extension.NodeId(associateId);

// For Matrix trees where an associate can appear multiple times
// var nodeId = new DirectScale.Disco.Extension.NodeId(associateId, treeIndex: 0);
```

## Determine the upline position

Identify where to place the associate:

```csharp
// Get the associate's current position in the Enrollment tree
var enrollmentNodeDetail = _treeService.GetNodeDetail(nodeId, TreeType.Enrollment);

// Get their enroller (upline in the Enrollment tree)
var uplineId = enrollmentNodeDetail.UplineId;
```

## Create the placement object

Define where and how to place the associate:

```csharp
var placement = new Placement 
{ 
    Tree = TreeType.Unilevel,  // Target tree type 
    NodeDetail = new NodeDetail 
    { 
        NodeId = nodeId,       // The associate to place
        UplineId = uplineId,   // Where to place them
        UplineLeg = LegName.Empty  // Position (Empty for Unilevel/Enrollment)
    }
};
```

!!! note 
    Use the appropriate LegName for your tree type:

    * Unilevel/Enrollment trees: `LegName.Empty`
    * Binary trees: `LegName.Left` or `LegName.Right`
    * Matrix trees: `LegName.Position1`, `LegName.Position2`, etc.

## Validate the placement

Always validate placements before executing them:

```csharp
try
{
    _treeService.ValidatePlacements(new[] { placement });
}
catch (Exception ex)
{
    // Handle validation errors
    Console.WriteLine($"Placement validation failed: {ex.Message}");
    return; // Don't proceed if validation fails
}
```

## Execute the placement

After validation, place the associate:

```csharp
try
{
    _treeService.Place(new[] { placement });
    Console.WriteLine("Placement successful");
}
catch (Exception ex)
{
    Console.WriteLine($"Placement failed: {ex.Message}");
    // Implement appropriate error handling
}
```

This example places an associate in the Unilevel tree under their enroller:

```csharp
public void PlaceAssociateUnderEnroller(int associateId)
{
    try
    {
        // Create the NodeId
        var nodeId = new DirectScale.Disco.Extension.NodeId(associateId);
        
        // Get enrollment details to find the enroller
        var enrollmentNodeDetail = _treeService.GetNodeDetail(nodeId, TreeType.Enrollment);
        
        // Create the placement object
        var placement = new Placement 
        { 
            Tree = TreeType.Unilevel, 
            NodeDetail = new NodeDetail 
            { 
                NodeId = nodeId, 
                UplineId = enrollmentNodeDetail.UplineId, 
                UplineLeg = LegName.Empty 
            } 
        };
        
        // Validate first
        _treeService.ValidatePlacements(new[] { placement });
        
        // Execute the placement
        _treeService.Place(new[] { placement });
        
        Console.WriteLine($"Associate {associateId} successfully placed in Unilevel tree");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error placing associate in tree: {ex.Message}");
        // Additional error handling as appropriate
    }
}
```

## Verify tree placement

After placement, verify the associate's position:

```csharp
public bool VerifyPlacement(int associateId, int expectedUplineId, TreeType treeType)
{
    var nodeId = new DirectScale.Disco.Extension.NodeId(associateId);
    var nodeDetail = _treeService.GetNodeDetail(nodeId, treeType);
    
    return nodeDetail.UplineId.AssociateId == expectedUplineId;
}
```

## Common scenarios

### Place in a Binary Tree with specific leg

```csharp
var binaryPlacement = new Placement 
{ 
    Tree = TreeType.Binary, 
    NodeDetail = new NodeDetail 
    { 
        NodeId = nodeId, 
        UplineId = targetUplineId, 
        UplineLeg = LegName.Right  // or LegName.Left
    } 
};
```

### Place multiple associates at once

```csharp
var placements = new[] 
{
    new Placement { Tree = TreeType.Unilevel, NodeDetail = /* details for first associate */ },
    new Placement { Tree = TreeType.Unilevel, NodeDetail = /* details for second associate */ }
};

_treeService.ValidatePlacements(placements);
_treeService.Place(placements);
```

## Troubleshooting

* **Circular reference errors**: ensure you're not creating loops in the tree structure.
* **Invalid leg errors**: verify you're using the correct LegName for the tree type.
* **Position already filled**: check if the target position already contains another associate.
* **TreeType mismatch**: confirm the TreeType is consistent between validation and placement.
