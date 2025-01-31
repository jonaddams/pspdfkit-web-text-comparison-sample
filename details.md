# Technical Implementation Details

## Document Processing Workflow

See the [Nutrient Guide](https://www.nutrient.io/guides/web/comparison/compare-text/#programmatic-text-comparison) for More Information

### 1. Initialization Process

```
Load Original PDF ──┐
                   ├──► Set up synchronized viewers
Load Changed PDF ───┘
```

### 2. Page-by-Page Processing

```
For each page:
┌─► Create document descriptors for both PDFs
│   │
│   ├─► Get page information (dimensions, etc.)
│   │
│   ├─► Perform text comparison
│   │   │
│   │   └─► For each difference found:
│   │       ├─► Process deletions (mark in red)
│   │       └─► Process insertions (mark in blue)
│   │
│   ├─► Create highlight annotations
│   │
│   └─► Update operations map for sidebar
│
└─► Move to next page
```

### 3. Document Changes Tracking Structure

```javascript
Operation Map Structure:
{
  "coordinate": {
    deleteText: "removed text",
    insertText: "added text",
    del: boolean,
    insert: boolean
  }
}
A deletion and insertion is a replacement
```

### 4. UI Update Flow

```
Load SDK, Load Documents ──► Calculate Text Differences Per Page ──► Add Highlight Annotations Per Page ──► Update Operations Map Per Page ──► Trigger Re-render After Last Page ──► Update Sidebar Component
```

## Key Implementation Features

1.  **Change Detection**

- Low-level text comparison
- Configurable context word count
- Efficient diff algorithm implementation

2. **Coordinate System**

   - Coordinate-based annotation for accurate difference highlighting
   - Automatic adjustment for different page sizes and orientations

3. **Memory Management**
   - Page-by-page processing prevents memory issues with large documents
   - Efficient garbage collection through proper cleanup
