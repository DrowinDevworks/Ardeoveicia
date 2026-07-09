GEMINI CHAT CORE
<https://share.gemini.google/MwPbM93eArDG>

JSON
```
{
  "id": "1",
  "name": "Save Project",
  "keys": ["Control", "Shift", "S"],
  "notes": "Saves the current state to the cloud."
}
```

```
CSS
.keycap {
  display: inline-block;
  padding: 4px 8px;
  background-color: #f3f4f6;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  box-shadow: 0 2px 0 #d1d5db;
  font-family: monospace;
  font-weight: 600;
  text-transform: capitalize;
}
```

HTML
```
<style>
  body { font-family: sans-serif; padding: 2rem; }
  .row { display: flex; gap: 1rem; margin-bottom: 1rem; align-items: center; }
  input, .recorder { padding: 0.5rem; border: 1px solid #ccc; border-radius: 4px; }
  .recorder {
    min-width: 200px;
    min-height: 24px;
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    background: #fff;
  }
  .recorder:focus { outline: 2px solid #3b82f6; background: #f9fafb; }
  .recorder:empty::before { content: "Click to record..."; color: #9ca3af; }
  
  /* The Keycap Style */
  .keycap {
    display: inline-block;
    padding: 2px 6px;
    background-color: #f3f4f6;
    border: 1px solid #d1d5db;
    border-radius: 4px;
    box-shadow: 0 2px 0 #d1d5db;
    font-family: monospace;
    font-weight: 600;
  }
</style>

<div class="row">
  <input type="text" placeholder="Shortcut Name (e.g. Save)" />
  <div class="recorder" id="shortcut-recorder" tabindex="0"></div>
  <input type="text" placeholder="Notes" />
</div>
```

```
JAVASCRIPT
const recorder = document.getElementById('shortcut-recorder');
let currentKeys = [];

recorder.addEventListener('keydown', (e) => {
  // 1. Stop the browser from executing the shortcut (e.g., Ctrl+S)
  e.preventDefault();

  const modifiers = [];
  
  // 2. Catch modifiers first for logical order
  if (e.ctrlKey) modifiers.push('Ctrl');
  if (e.shiftKey) modifiers.push('Shift');
  if (e.altKey) modifiers.push('Alt');
  if (e.metaKey) modifiers.push('Cmd'); // or Win

  // 3. Catch the main key
  const key = e.key;
  // Ignore if the user ONLY pressed a modifier key so far
  const isModifier = ['Control', 'Shift', 'Alt', 'Meta'].includes(key);

  currentKeys = [...modifiers];
  
  if (!isModifier) {
    // Format the key nicely (e.g., "s" becomes "S", " " becomes "Space")
    let formattedKey = key.length === 1 ? key.toUpperCase() : key;
    if (formattedKey === ' ') formattedKey = 'Space';
    
    currentKeys.push(formattedKey);
  }

  // 4. Render to the DOM
  renderKeys();
});

function renderKeys() {
  if (currentKeys.length === 0) {
    recorder.innerHTML = '';
    return;
  }
  
  // Create an array of HTML strings and join them with a plus sign
  const htmlArr = currentKeys.map(k => `<span class="keycap">${k}</span>`);
  recorder.innerHTML = htmlArr.join(' <span style="color:#6b7280">+</span> ');
}
```
