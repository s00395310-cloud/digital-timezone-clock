const timeZones = [
  { city: 'UTC', zone: 'UTC' },
  { city: 'New York', zone: 'America/New_York' },
  { city: 'London', zone: 'Europe/London' },
  { city: 'Paris', zone: 'Europe/Paris' },
  { city: 'Dubai', zone: 'Asia/Dubai' },
  { city: 'Tokyo', zone: 'Asia/Tokyo' },
  { city: 'Sydney', zone: 'Australia/Sydney' },
  { city: 'Los Angeles', zone: 'America/Los_Angeles' }
];

const clockGrid = document.getElementById('clockGrid');

function formatTime(date, timeZone) {
  const options = {
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit',
    hour12: false,
    timeZone
  };

  return new Intl.DateTimeFormat('en-GB', options).format(date);
}

function formatDate(date, timeZone) {
  const options = {
    weekday: 'short',
    month: 'short',
    day: 'numeric',
    year: 'numeric',
    timeZone
  };

  return new Intl.DateTimeFormat('en-US', options).format(date);
}

function getTimeZoneOffset(date, timeZone) {
  const formatter = new Intl.DateTimeFormat('en-US', {
    timeZone,
    timeZoneName: 'shortOffset',
    hour: '2-digit',
    minute: '2-digit',
    hour12: false
  });

  const parts = formatter.formatToParts(date);
  const tzName = parts.find(part => part.type === 'timeZoneName')?.value || 'UTC';
  return tzName.replace('GMT', 'UTC');
}

function renderClocks() {
  const now = new Date();

  clockGrid.innerHTML = timeZones
    .map(({ city, zone }) => {
      const time = formatTime(now, zone);
      const date = formatDate(now, zone);
      const offset = getTimeZoneOffset(now, zone);

      return `
        <article class="clock-card">
          <div class="city">${city}</div>
          <div class="time">${time}</div>
          <div class="date">${date}</div>
          <div class="offset">${offset}</div>
        </article>
      `;
    })
    .join('');
}

renderClocks();
setInterval(renderClocks, 1000);
