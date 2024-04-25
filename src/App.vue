<template>
  <div
    id="app"
    class="flex min-h-screen min-w-full flex-col gap-y-2 bg-gray-200 p-8 text-gray-800 dark:bg-gray-900 dark:text-gray-200 sm:p-12 md:p-16 lg:px-20 lg:py-10 xl:px-24 xl:py-12 2xl:px-32 2xl:py-16"
  >
    <h1 class="text-4xl font-semibold">MIDI Message Reader</h1>
    <p class="text-gray-600 dark:text-gray-400">
      Translate MIDI hexadecimal messages into something more human readable.
    </p>
    <div
      class="mt-2 flex flex-col gap-y-2 rounded-lg bg-gray-100 p-4 dark:bg-gray-800 md:w-fit"
    >
      <span class="text-sm text-gray-500">Hexadecimal input here...</span>
      <div class="flex flex-col gap-x-2 gap-y-2 sm:flex-row">
        <input
          v-model="status"
          type="text"
          maxlength="2"
          class="form-input w-full rounded-md border-2 border-gray-300 bg-gray-100 p-2 focus:bg-gray-200 focus:ring-0 dark:border-gray-700 dark:bg-gray-800 dark:focus:bg-gray-900 md:w-48"
          placeholder="Status Byte (e.g. 9F)"
        />
        <input
          v-model="data1"
          type="text"
          maxlength="2"
          class="form-input w-full rounded-md border-2 border-gray-300 bg-gray-100 p-2 focus:bg-gray-200 focus:ring-0 dark:border-gray-700 dark:bg-gray-800 dark:focus:bg-gray-900 md:w-48"
          placeholder="Data Byte 1 (e.g. 0C)"
        />
        <input
          v-model="data2"
          maxlength="2"
          type="text"
          class="form-input w-full rounded-md border-2 border-gray-300 bg-gray-100 p-2 focus:bg-gray-200 focus:ring-0 dark:border-gray-700 dark:bg-gray-800 dark:focus:bg-gray-900 md:w-48"
          placeholder="Data Byte 2 (e.g. 00)"
        />
      </div>
    </div>
    <div class="mt-2 flex flex-col gap-y-2" v-if="status">
      <span class="text-sm text-gray-500">This input means...</span>
      <div class="flex flex-col gap-y-2">
        <div>
          <div v-if="status.length === 1">
            <p v-if="status.slice(0, 1).toLowerCase() === '8'">Note Off</p>
            <p v-else-if="status.slice(0, 1).toLowerCase() === '9'">Note On</p>
            <p v-else-if="status.slice(0, 1).toLowerCase() === 'a'">
              Polyphonic Aftertouch
            </p>
            <p v-else-if="status.slice(0, 1).toLowerCase() === 'b'">
              Control/Mode Change
            </p>
            <p v-else-if="status.slice(0, 1).toLowerCase() === 'c'">
              Program Change
            </p>
            <p v-else-if="status.slice(0, 1).toLowerCase() === 'd'">
              Channel Aftertouch
            </p>
            <p v-else-if="status.slice(0, 1).toLowerCase() === 'e'">
              Pitch Bend
            </p>
            <p v-else-if="status.slice(0, 1).toLowerCase() === 'f'">
              SysEx Message
            </p>
            <p v-else>Status {{ status.toUpperCase() }}</p>
          </div>
          <div v-else-if="status.length === 2">
            <div v-if="status.slice(0, 1).toLowerCase() === '8'">
              <p>
                Note Off @ Channel {{ parseInt(status.slice(1, 2), 16) + 1 }}
              </p>
            </div>
            <div v-else-if="status.slice(0, 1).toLowerCase() === '9'">
              <p>
                Note On @ Channel {{ parseInt(status.slice(1, 2), 16) + 1 }}
              </p>
            </div>
            <div v-else-if="status.slice(0, 1).toLowerCase() === 'a'">
              <p>
                Polyphonic Aftertouch @ Channel
                {{ parseInt(status.slice(1, 2), 16) + 1 }}
              </p>
            </div>
            <div v-else-if="status.slice(0, 1).toLowerCase() === 'b'">
              <p>
                Control/Mode Change @ Channel
                {{ parseInt(status.slice(1, 2), 16) + 1 }}
              </p>
            </div>
            <div v-else-if="status.slice(0, 1).toLowerCase() === 'c'">
              <p>
                Program Change @ Channel
                {{ parseInt(status.slice(1, 2), 16) + 1 }}
              </p>
            </div>
            <div v-else-if="status.slice(0, 1).toLowerCase() === 'd'">
              <p>
                Channel Aftertouch @ Channel
                {{ parseInt(status.slice(1, 2), 16) + 1 }}
              </p>
            </div>
            <div v-else-if="status.slice(0, 1).toLowerCase() === 'e'">
              <p>
                Pitch Bend @ Channel
                {{ parseInt(status.slice(1, 2), 16) + 1 }}
              </p>
            </div>
            <div v-else-if="status.slice(0, 1).toLowerCase() === 'f'">
              <p v-if="status.slice(1, 2).toLowerCase() === '0'">
                System Exclusive
              </p>
              <p v-else-if="status.slice(1, 2).toLowerCase() === '1'">
                MIDI Time Code Qtr. Frame
              </p>
              <p v-else-if="status.slice(1, 2).toLowerCase() === '2'">
                Song Position Pointer
              </p>
              <p v-else-if="status.slice(1, 2).toLowerCase() === '3'">
                Song Select (Song #)
              </p>
              <p v-else-if="status.slice(1, 2).toLowerCase() === '6'">
                Tune Request
              </p>
              <p v-else-if="status.slice(1, 2).toLowerCase() === '7'">
                End of SysEx Message (EOX)
              </p>
              <p v-else-if="status.slice(1, 2).toLowerCase() === '8'">
                Timing Clock
              </p>
              <p v-else-if="status.slice(1, 2).toLowerCase() === 'a'">Start</p>
              <p v-else-if="status.slice(1, 2).toLowerCase() === 'b'">
                Continue
              </p>
              <p v-else-if="status.slice(1, 2).toLowerCase() === 'c'">Stop</p>
              <p v-else-if="status.slice(1, 2).toLowerCase() === 'e'">
                Active Sensing
              </p>
              <p v-else-if="status.slice(1, 2).toLowerCase() === 'f'">
                System Reset
              </p>
              <p v-else>Status {{ status.toUpperCase() }}</p>
            </div>
            <div v-else>Status {{ status.toUpperCase() }}</div>
          </div>
          <p v-else class="text-gray-400"></p>
        </div>
        <div v-if="status.length === 2">
          <div
            v-if="
              status.slice(0, 1).toLowerCase() === '8' ||
              status.slice(0, 1).toLowerCase() === '9' ||
              status.slice(0, 1).toLowerCase() === 'a'
            "
          >
            <p v-if="data1">
              Note {{ parseInt(data1, 16) }}
              <span v-if="parseInt(data1, 16) < 128"
                >({{ notes[parseInt(data1, 16)] }})</span
              >
            </p>
          </div>
          <div v-else-if="status.slice(0, 1).toLowerCase() === 'b'">
            <p v-if="data1">
              <span v-if="cc[parseInt(data1, 16)].func === null"
                >Undefined
              </span>
              <span v-else>{{ cc[parseInt(data1, 16)].func }}</span>
            </p>
          </div>
          <div v-else-if="status.slice(0, 1).toLowerCase() === 'c'">
            <p>
              Program
              {{ parseInt(data1, 16) }} (Range 0-127)
            </p>
          </div>
          <div v-else-if="status.slice(0, 1).toLowerCase() === 'd'">
            <p>
              Pressure {{ parseInt(data1, 16) }} ({{
                Math.round((parseInt(data1, 16) / 127) * 100 * 10) / 10
              }}%)
            </p>
          </div>
          <div v-else-if="status.slice(0, 1).toLowerCase() === 'e'">
            <p>
              LSB Value {{ parseInt(data1, 16) }} ({{
                Math.round((parseInt(data1, 16) / 127) * 100 * 10) / 10
              }}%)
            </p>
          </div>
          <div v-else-if="status.slice(0, 1).toLowerCase() === 'f'">
            <p v-if="status.slice(1, 2).toLowerCase() === '2'">
              LSB Value {{ parseInt(data1, 16) }} ({{
                Math.round((parseInt(data1, 16) / 127) * 100 * 10) / 10
              }}%)
            </p>
            <p v-else-if="status.slice(1, 2).toLowerCase() === '3'">
              System Value {{ parseInt(data1, 16) }} (Range 0-127)
            </p>
            <p v-else></p>
          </div>
          <div v-else></div>
        </div>
        <div v-if="status.length === 2">
          <div
            v-if="
              status.slice(0, 1).toLowerCase() === '8' ||
              status.slice(0, 1).toLowerCase() === '9'
            "
          >
            <p v-if="data2">
              Velocity {{ parseInt(data2, 16) }} ({{
                Math.round((parseInt(data2, 16) / 127) * 100 * 10) / 10
              }}%)
            </p>
          </div>
          <div v-else-if="status.slice(0, 1).toLowerCase() === 'a'">
            <p v-if="data2">
              Pressure {{ parseInt(data2, 16) }} ({{
                Math.round((parseInt(data2, 16) / 127) * 100 * 10) / 10
              }}%)
            </p>
          </div>
          <div v-else-if="status.slice(0, 1).toLowerCase() === 'b'">
            <div v-if="data2">
              <p
                v-if="
                  data1 == 40 ||
                  data1 == 41 ||
                  data1 == 42 ||
                  data1 == 43 ||
                  data1 == 45
                "
              >
                {{ cc[parseInt(data1, 16)].use }} Value
                {{ parseInt(data2, 16) }} (<span
                  v-if="parseInt(data2, 16) <= 63"
                  >Off</span
                ><span v-else-if="parseInt(data2, 16) >= 64">On</span>)
              </p>
              <p v-else-if="data1 == 44">
                {{ cc[parseInt(data1, 16)].use }} Value
                {{ parseInt(data2, 16) }} (<span
                  v-if="parseInt(data2, 16) <= 63"
                  >Normal</span
                ><span v-else-if="parseInt(data2, 16) >= 64">Legato</span>)
              </p>
              <p v-else-if="data1.toLowerCase() == '7a'">
                {{ cc[parseInt(data1, 16)].use }} Value
                {{ parseInt(data2, 16) }}
                <span v-if="parseInt(data2, 16) === 0">(Off)</span
                ><span v-else-if="parseInt(data2, 16) === 127">(On)</span>
              </p>
              <p v-else>
                <span>
                  <span v-if="cc[parseInt(data1, 16)].use !== null">{{
                    cc[parseInt(data1, 16)].use
                  }}</span>
                  <span v-else>CC</span>
                </span>
                Value {{ parseInt(data2, 16) }}
                <span v-if="cc[parseInt(data1, 16)].rangeMin !== null"
                  >(Range {{ cc[parseInt(data1, 16)].rangeMin }}-{{
                    cc[parseInt(data1, 16)].rangeMax
                  }})</span
                >
              </p>
            </div>
          </div>
          <div v-else-if="status.slice(0, 1).toLowerCase() === 'e'">
            <p v-if="data2">
              MSB Value {{ parseInt(data2, 16) }} ({{
                Math.round((parseInt(data2, 16) / 127) * 100 * 10) / 10
              }}%)
            </p>
          </div>
          <div v-else-if="status.slice(0, 1).toLowerCase() === 'f'">
            <p v-if="status.slice(1, 2).toLowerCase() === '2'">
              MSB Value {{ parseInt(data2, 16) }} ({{
                Math.round((parseInt(data2, 16) / 127) * 100 * 10) / 10
              }}%)
            </p>
            <p v-else></p>
          </div>
          <div v-else></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      status: "",
      data1: "",
      data2: "",
      notes: [
        "C-2",
        "C♯-2 / D♭-2",
        "D-2",
        "D♯-2 / E♭-2",
        "E-2",
        "F-2",
        "F♯-2 / G♭-2",
        "G-2",
        "G♯-2 / A♭-2",
        "A-2",
        "A♯-2 / B♭-2",
        "B-2",
        "C-1",
        "C♯-1 / D♭-1",
        "D-1",
        "D♯-1 / E♭-1",
        "E-1",
        "F-1",
        "F♯-1 / G♭-1",
        "G-1",
        "G♯-1 / A♭-1",
        "A-1",
        "A♯-1 / B♭-1",
        "B-1",
        "C0",
        "C♯0 / D♭0",
        "D0",
        "D♯0 / E♭0",
        "E0",
        "F0",
        "F♯0 / G♭0",
        "G0",
        "G♯0 / A♭0",
        "A0",
        "A♯0 / B♭0",
        "B0",
        "C1",
        "C♯1 / D♭1",
        "D1",
        "D♯1 / E♭1",
        "E1",
        "F1",
        "F♯1 / G♭1",
        "G1",
        "G♯1 / A♭1",
        "A1",
        "A♯1 / B♭1",
        "B1",
        "C2",
        "C♯2 / D♭2",
        "D2",
        "D♯2 / E♭2",
        "E2",
        "F2",
        "F♯2 / G♭2",
        "G2",
        "G♯2 / A♭2",
        "A2",
        "A♯2 / B♭2",
        "B2",
        "C3",
        "C♯3 / D♭3",
        "D3",
        "D♯3 / E♭3",
        "E3",
        "F3",
        "F♯3 / G♭3",
        "G3",
        "G♯3 / A♭3",
        "A3",
        "A♯3 / B♭3",
        "B3",
        "C4",
        "C♯4 / D♭4",
        "D4",
        "D♯4 / E♭4",
        "E4",
        "F4",
        "F♯4 / G♭4",
        "G4",
        "G♯4 / A♭4",
        "A4",
        "A♯4 / B♭4",
        "B4",
        "C5",
        "C♯5 / D♭5",
        "D5",
        "D♯5 / E♭5",
        "E5",
        "F5",
        "F♯5 / G♭5",
        "G5",
        "G♯5 / A♭5",
        "A5",
        "A♯5 / B♭5",
        "B5",
        "C6",
        "C♯6 / D♭6",
        "D6",
        "D♯6 / E♭6",
        "E6",
        "F6",
        "F♯6 / G♭6",
        "G6",
        "G♯6 / A♭6",
        "A6",
        "A♯6 / B♭6",
        "B6",
        "C7",
        "C♯7 / D♭7",
        "D7",
        "D♯7 / E♭7",
        "E7",
        "F7",
        "F♯7 / G♭7",
        "G7",
        "G♯7 / A♭7",
        "A7",
        "A♯7 / B♭7",
        "B7",
        "C8",
        "C♯8 / D♭8",
        "D8",
        "D♯8 / E♭8",
        "E8",
        "F8",
        "F♯8 / G♭8",
        "G8",
      ],
      cc: [
        {
          func: "Bank Select",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "Modulation Wheel",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "Breath Controller",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "Foot Controller",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "Portamento Time",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "Data Entry",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "Channel Volume (formerly Main Volume)",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "Balance",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "Pan",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "Expression Controller",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "Effect Control 1",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "Effect Control 2",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "General Purpose Controller 1",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "General Purpose Controller 2",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "General Purpose Controller 3",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "General Purpose Controller 4",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "Bank Select",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Modulation Wheel",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Breath Controller",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Foot Controller",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Portamento Time",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Data Entry",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Channel Volume (formerly Main Volume)",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Balance",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Pan",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Expression Controller",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Effect Control 1",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Effect Control 2",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "General Purpose Controller 1",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "General Purpose Controller 2",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "General Purpose Controller 3",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "General Purpose Controller 4",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Sustain/Damper Pedal",
          rangeMin: 63,
          rangeMax: 64,
          use: "On/Off",
        },
        {
          func: "Portamento",
          rangeMin: 63,
          rangeMax: 64,
          use: "On/Off",
        },
        {
          func: "Sostenuto",
          rangeMin: 63,
          rangeMax: 64,
          use: "On/Off",
        },
        {
          func: "Soft Pedal",
          rangeMin: 63,
          rangeMax: 64,
          use: "On/Off",
        },
        {
          func: "Legato Footswitch",
          rangeMin: 63,
          rangeMax: 64,
          use: "On/Off",
        },
        {
          func: "Hold 2",
          rangeMin: 63,
          rangeMax: 64,
          use: "On/Off",
        },
        {
          func: "Sound Controller 1, default: Sound Variation",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Sound Controller 2, default: Timbre/Harmonic Intensity",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Sound Controller 3, default: Release Time",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Sound Controller 4, default: Attack Time",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Sound Controller 5, default: Brightness",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Sound Controller 6, default: Decay Time",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Sound Controller 7, default: Vibrato Rate",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Sound Controller 8, default: Vibrato Depth",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Sound Controller 9, default: Vibrato Delay",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Sound Controller 10, default: Undefined",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "General Purpose Controller 5",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "General Purpose Controller 6",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "General Purpose Controller 7",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "General Purpose Controller 8",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Portamento Control",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: "High Resolution Velocity Prefix",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: "Effect 1 Depth, default: Reverb Send Level (formerly External Effects Depth)",
          rangeMin: 0,
          rangeMax: 127,
          use: null,
        },
        {
          func: "Effect 2 Depth (formerly Tremolo Depth)",
          rangeMin: 0,
          rangeMax: 127,
          use: null,
        },
        {
          func: "Effect 3 Depth, default: Chorus Send Level (formerly Chorus Depth)",
          rangeMin: 0,
          rangeMax: 127,
          use: null,
        },
        {
          func: "Effect 4 Depth (formerly Celeste [Detune] Depth)",
          rangeMin: 0,
          rangeMax: 127,
          use: null,
        },
        {
          func: "Effect 5 Depth (formerly Phaser Depth)",
          rangeMin: 0,
          rangeMax: 127,
          use: null,
        },
        {
          func: "Data Increment (Data Entry +1)",
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: "Data Decrement (Data Entry -1)",
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: "Non-Registered Parameter Number",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Non-Registered Parameter Number",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: "Registered Parameter Number",
          rangeMin: 0,
          rangeMax: 127,
          use: "LSB",
        },
        {
          func: "Registered Parameter Number",
          rangeMin: 0,
          rangeMax: 127,
          use: "MSB",
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: null,
          rangeMin: null,
          rangeMax: null,
          use: null,
        },
        {
          func: "All Sound Off",
          rangeMin: 0,
          rangeMax: 0,
          use: null,
        },
        {
          func: "Reset All Controllers",
          rangeMin: 0,
          rangeMax: 0,
          use: null,
        },
        {
          func: "Local Control",
          rangeMin: 0,
          rangeMax: 127,
          use: "On/Off",
        },
        {
          func: "All Notes Off",
          rangeMin: 0,
          rangeMax: 0,
          use: null,
        },
        {
          func: "Omni Mode Off",
          rangeMin: 0,
          rangeMax: 0,
          use: null,
        },
        {
          func: "Omni Mode On",
          rangeMin: 0,
          rangeMax: 0,
          use: null,
        },
        {
          func: "Mono Mode On",
          rangeMin: 0,
          rangeMax: 0,
          use: null,
        },
        {
          func: "Poly Mode On",
          rangeMin: 0,
          rangeMax: 0,
          use: null,
        },
      ],
    };
  },
};
</script>
