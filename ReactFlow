import { useState } from 'react';

const ReactFlowDemo = () => {
  const [nodes, setNodes] = useState([
    { id: '1', label: 'Action 01', x: 250, y: 5 },
    { id: '2', label: 'Action 02', x: 250, y: 100 }
  ]);
  const [edges, setEdges] = useState([]);
  const [isDragging, setIsDragging] = useState(false);
  const [dragItem, setDragItem] = useState(null);
  
  // Handle node drop
  const handleDrop = (e, x, y) => {
    e.preventDefault();
    if (dragItem) {
      const newId = `dndnode_${nodes.length}`;
      setNodes([...nodes, { id: newId, label: `${dragItem} node`, x, y }]);
      setIsDragging(false);
    }
  };
  
  // Connect nodes
  const connectNodes = (sourceId, targetId) => {
    if (sourceId && targetId && sourceId !== targetId) {
      const newEdge = { id: `e${sourceId}-${targetId}`, source: sourceId, target: targetId };
      // Check if edge already exists
      if (!edges.some(e => e.source === sourceId && e.target === targetId)) {
        setEdges([...edges, newEdge]);
      }
    }
  };
  
  return (
    <div className="flex w-full h-screen bg-gray-100">
      {/* Sidebar */}
      <div className="w-64 p-4 bg-white border-r border-gray-200">
        <div className="mb-4 text-sm text-gray-600">You can drag these nodes to the pane on the right.</div>
        <div 
          className="p-2 mb-2 bg-blue-100 rounded cursor-move border border-blue-200 text-center hover:bg-blue-200"
          draggable
          onDragStart={() => {
            setIsDragging(true);
            setDragItem('Action 01');
          }}
        >
          Action 01
        </div>
        <div 
          className="p-2 mb-2 bg-green-100 rounded cursor-move border border-green-200 text-center hover:bg-green-200"
          draggable
          onDragStart={() => {
            setIsDragging(true);
            setDragItem('Action 02');
          }}
        >
          Action 02
        </div>
      </div>
      
      {/* Flow Canvas */}
      <div 
        className="flex-1 relative"
        onDragOver={(e) => e.preventDefault()}
        onClick={(e) => {
          // For demo purposes - allow clicking on canvas to place node
          if (isDragging) {
            const rect = e.currentTarget.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;
            handleDrop(e, x, y);
          }
        }}
      >
        {/* Background pattern */}
        <div className="absolute inset-0" style={{
          backgroundImage: 'radial-gradient(#e5e7eb 1px, transparent 1px)',
          backgroundSize: '20px 20px'
        }}></div>
        
        {/* Nodes */}
        {nodes.map((node) => (
          <div
            key={node.id}
            className="absolute p-2 border bg-white rounded shadow-md cursor-pointer hover:shadow-lg"
            style={{ left: node.x, top: node.y, zIndex: 10 }}
            onClick={() => {
              // Demo connection on click - in real app this would use handles
              const selectedNodeId = localStorage.getItem('selectedNode');
              if (selectedNodeId && selectedNodeId !== node.id) {
                connectNodes(selectedNodeId, node.id);
                localStorage.removeItem('selectedNode');
              } else {
                localStorage.setItem('selectedNode', node.id);
              }
            }}
          >
            {node.label}
          </div>
        ))}
        
        {/* Edges - simplified SVG representation */}
        <svg className="absolute inset-0 pointer-events-none" style={{zIndex: 5}}>
          {edges.map((edge) => {
            const source = nodes.find(n => n.id === edge.source);
            const target = nodes.find(n => n.id === edge.target);
            if (source && target) {
              // Calculate center points of nodes
              const sourceX = source.x + 50;
              const sourceY = source.y + 15;
              const targetX = target.x + 50;
              const targetY = target.y + 15;
              
              return (
                <g key={edge.id}>
                  <path
                    d={`M${sourceX},${sourceY} C${sourceX},${(sourceY + targetY)/2} ${targetX},${(sourceY + targetY)/2} ${targetX},${targetY}`}
                    stroke="#b1b1b7"
                    strokeWidth="2"
                    fill="none"
                  />
                  {/* Arrow head */}
                  <circle cx={targetX} cy={targetY} r="3" fill="#b1b1b7" />
                </g>
              );
            }
            return null;
          })}
        </svg>
        
        {/* Controls */}
        <div className="absolute bottom-4 right-4 flex space-x-2">
          <button className="p-2 bg-white rounded shadow hover:bg-gray-100">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
              <circle cx="12" cy="12" r="10"/>
              <line x1="12" y1="8" x2="12" y2="16"/>
              <line x1="8" y1="12" x2="16" y2="12"/>
            </svg>
          </button>
          <button className="p-2 bg-white rounded shadow hover:bg-gray-100">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
              <circle cx="12" cy="12" r="10"/>
              <line x1="8" y1="12" x2="16" y2="12"/>
            </svg>
          </button>
          <button className="p-2 bg-white rounded shadow hover:bg-gray-100">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
              <polyline points="15 18 9 12 15 6"/>
            </svg>
          </button>
        </div>
        
        <div className="absolute top-4 left-4 p-2 text-gray-600 text-sm bg-white bg-opacity-70 rounded">
          <p>Click on a node to select it, then click another node to connect them</p>
          <p>Click anywhere on the canvas while dragging to place a new node</p>
        </div>
      </div>
    </div>
  );
};

export default ReactFlowDemo;
