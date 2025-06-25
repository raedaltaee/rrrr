const api = 'http://localhost:3000/api/properties';

async function addProperty() {
  const name = document.getElementById('name').value;
  const location = document.getElementById('location').value;
  const type = document.getElementById('type').value;
  const value = document.getElementById('value').value;
  const notes = document.getElementById('notes').value;
  if(!name) { alert('يرجى كتابة اسم الملك!'); return; }
  await fetch(api, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ name, location, type, value, notes })
  });
  document.getElementById('name').value = '';
  document.getElementById('location').value = '';
  document.getElementById('type').value = '';
  document.getElementById('value').value = '';
  document.getElementById('notes').value = '';
  loadProperties();
}

async function loadProperties() {
  const res = await fetch(api);
  const data = await res.json();
  document.getElementById('list').innerHTML = data.map(i =>
    `<li>
      <b>${i.name}</b> - ${i.location || ''} - ${i.type || ''} - ${i.value || ''} <i>(${i.notes || ''})</i>
      <button onclick="deleteProperty('${i._id}')">حذف</button>
    </li>`
