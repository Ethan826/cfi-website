---
title: RDU Airport
menus: main
---

[Raleigh Flying Club](https://www.raleighflyingclub.org/), where I instruct, is
located at Raleigh-Durham International Airport.

# Why RDU?

Here are some advantages to training at RDU:

- **Location:** The club’s central location is convenient to Apex, Cary,
  Raleigh, Durham, Chapel Hill, and other communities in the Triangle.

- **Confidence:** Learning to fly at a busy, tower-controlled airport like RDU
  can be a significant confidence booster. While less busy fields offer
  efficiency, training in a bustling environment equips you with the skills to
  handle complex airspace. This experience prepares you to fly confidently
  almost anywhere, whether for fun or as a career and ensures you are
  well-practiced in communicating with air traffic control.

- **Access to Diverse Training Environments:** RDU’s proximity to various
  practice areas and smaller airports allows for versatile training sessions.
  You can quickly transition from busy airspace to quieter environments to hone
  your skills on different maneuvers, offering the best of both worlds.

# Resources

- [Weather](https://aviationweather.gov/data/metar/?id=KRDU&hours=0&decoded=yes&include_taf=yes)
- <a id="airport-diagram" href="#">Loading Airport Diagram...</a>
- [Sectional Chart](https://skyvector.com/?ll=35.877638889,-78.787472222&chart=301&zoom=1)
- [Directions](https://maps.app.goo.gl/LE8cUDQL2qd35NGV7)

<script>
  "use strict";

  const AIRAC_CONFIG = {
    FIRST_DAY: new Date("2016-01-07").getTime(),
    CYCLE_LENGTH_MS: 28 * 24 * 60 * 60 * 1000, // 28 days in milliseconds
    TOTAL_CYCLES: 326,
  };

  const generateAiracUrl = (airacCycle, airportCode = "RDU") =>
    `https://aeronav.faa.gov/d-tpp/${airacCycle}/00516ad.pdf#nameddest=(${airportCode})`;

  const computeAiracCycles = (totalCycles = AIRAC_CONFIG.TOTAL_CYCLES, dateConstructor = Date) => {
    const airacMap = new Map();
    for (let i = 0; i < totalCycles; i++) {
      const cycleStartDate = new dateConstructor(AIRAC_CONFIG.FIRST_DAY + AIRAC_CONFIG.CYCLE_LENGTH_MS * i);
      const year = cycleStartDate.getFullYear();
      if (!airacMap.has(year)) {
        airacMap.set(year, []);
      }
      airacMap.get(year).push(cycleStartDate);
    }
    return airacMap;
  };

  const cachedAiracCycles = computeAiracCycles();

  const getCurrentAiracCycle = (currentDate = new Date()) => {
    const year = currentDate.getFullYear();
    const yearCycles = cachedAiracCycles.get(year);

    if (!yearCycles) {
      throw new Error(`AIRAC cycle computation failed. No data for year ${year}. Update the range or check input.`);
    }

    const cycleNumber = yearCycles.filter(cycleStartDate => cycleStartDate.getTime() <= currentDate.getTime()).length;

    return `${year.toString().slice(2)}${cycleNumber.toString().padStart(2, "0")}`;
  };

  try {
    const currentAiracCycle = getCurrentAiracCycle();
    const airacUrl = generateAiracUrl(currentAiracCycle, "RDU");
    const diagramLink = document.getElementById("airport-diagram");
    diagramLink.textContent = "Airport diagram";
    diagramLink.setAttribute("href", airacUrl);
  } catch (error) {
    console.error("Error generating AIRAC URL:", error);
  }
</script>
