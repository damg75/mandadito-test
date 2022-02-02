<template>
  <v-container>
    <v-row dense>
      <v-col class="d-flex justify-space-between">
        <v-icon class="go-back-main" color="black" @click="step--"
          >mdi-keyboard-backspace</v-icon
        >
        <span class="font-weight-bold">Ridery | Encomiendas</span>
        <span color="white">.</span>
      </v-col>
    </v-row>

    <!-- step 0 -->
    <span v-if="step === 0">
      <v-row>
        <v-col>
          <!-- Origen y Destino -->
          <v-card class="pa-4">
            <v-row dense>
              <v-col>
                <h2 class="font-weight-bold">
                  Detalles de Envío <v-icon color="black">mdi-truck</v-icon>
                </h2>
              </v-col>
            </v-row>

            <v-row dense>
              <v-col>
                <v-img
                  src="@/assets/map.png"
                  alt="mapa"
                  max-height="400"
                  max-width="100%"
                ></v-img>
              </v-col>
            </v-row>
            <br />

            <!-- Origin -->
            <v-row dense>
              <v-col>
                <v-dialog
                  v-model="origin.dialog"
                  fullscreen
                  hide-overlay
                  transition="dialog-bottom-transition"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-bind="attrs"
                      v-on="on"
                      solo
                      placeholder="Dirección de Origen"
                      Label="Dirección de Origen"
                      color="cyan"
                      v-model="origin.origin"
                    >
                      <template v-slot:prepend-inner>
                        <v-icon v-on="on" color="cyan"> mdi-map-marker</v-icon>
                      </template>
                      <template v-slot:append>
                        <v-icon v-on="on" color="cyan"> mdi-pencil</v-icon>
                      </template>
                    </v-text-field>
                  </template>
                  <br />
                  <v-card>
                    <v-toolbar dark color="black">
                      <v-btn icon dark @click="origin.dialog = false">
                        <v-icon>mdi-keyboard-backspace</v-icon>
                      </v-btn>
                      <v-toolbar-title>Origen</v-toolbar-title>
                      <v-spacer></v-spacer>
                      <v-toolbar-items>
                        <v-btn dark text @click="handleSaveOrigin()">
                          Guardar
                        </v-btn>
                      </v-toolbar-items>
                    </v-toolbar>
                    <v-list three-line subheader>
                      <v-subheader>Detalles de Origen</v-subheader>
                      <v-list-item>
                        <v-list-item-content>
                          <v-list-item-title
                            >Especificar detalles de Origen</v-list-item-title
                          >
                          <v-list-item-subtitle
                            >Mensaje relacionado a detalles de origen <br />
                            <span class="text-decoration-underline"
                              >Más Información</span
                            >
                            <v-icon
                              >mdi-information-outline</v-icon
                            ></v-list-item-subtitle
                          >
                        </v-list-item-content>
                      </v-list-item>
                    </v-list>
                    <v-divider></v-divider>
                    <v-container>
                      <v-row dense>
                        <v-col>
                          <v-img
                            src="@/assets/map.png"
                            alt="mapa"
                            max-height="400"
                            max-width="100%"
                          ></v-img>
                        </v-col>
                      </v-row>
                      <v-row dense>
                        <v-col>
                          <v-text-field
                            solo
                            placeholder="Dirección de Origen"
                            label="Dirección de Origen"
                            color="cyan"
                            v-model="origin.origin"
                          >
                            <template v-slot:prepend-inner>
                              <v-icon v-on="on" color="cyan">
                                mdi-map-marker</v-icon
                              >
                            </template>
                          </v-text-field>
                        </v-col>
                      </v-row>
                      <v-row dense>
                        <v-col>
                          <label>Detalles de Origen</label>
                          <v-textarea
                            solo
                            placeholder="Juan va a bajar y entregará el paquete"
                            v-model="origin.details"
                          ></v-textarea>
                        </v-col>
                      </v-row>
                      <v-row dense>
                        <v-col>
                          <v-radio-group
                            v-model="origin.radioGroup"
                            class="d-flex flex-row"
                            label="Quién entrega el paquete?"
                          >
                            <v-radio label="Yo" value="Yo"></v-radio>
                            <v-radio label="Tercero" value="Tercero"></v-radio>
                          </v-radio-group>
                        </v-col>
                      </v-row>
                      <v-row dense v-if="origin.radioGroup === 'Tercero'">
                        <v-col>
                          <v-text-field
                            v-model="destination.thirdName"
                            label="Nombre de Tercero"
                            prepend-icon="mdi-account"
                          />
                        </v-col>
                        <v-col>
                          <v-text-field
                            v-model="origin.thirdPhone"
                            label="Número de Tercero"
                            type="number"
                            prepend-icon="mdi-cellphone"
                          />
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card>
                </v-dialog>
              </v-col>
            </v-row>
            <v-row dense v-if="origin.origin">
              <v-col>
                <v-expansion-panels v-model="panel" :disabled="disabled">
                  <v-expansion-panel>
                    <v-expansion-panel-header
                      >Detalle de Origen</v-expansion-panel-header
                    >
                    <v-expansion-panel-content>
                      <ul>
                        <li>Detalles: {{ origin.details }}</li>
                        <li>Persona a entregar: {{ origin.radioGroup }}</li>
                        <span v-if="origin.radioGroup === 'Tercero'">
                          <li>Número de Tercero: {{ origin.thirdPhone }}</li>
                          <li>Nombre de Tercero: {{ origin.thirdName }}</li>
                        </span>
                      </ul>
                    </v-expansion-panel-content>
                  </v-expansion-panel>
                </v-expansion-panels>
              </v-col>
            </v-row>
            <span v-if="origin.origin">
              <br />
              <br />
            </span>

            <!-- Destino -->
            <v-row dense>
              <v-col>
                <v-dialog
                  v-model="destination.dialog"
                  fullscreen
                  hide-overlay
                  transition="dialog-bottom-transition"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-bind="attrs"
                      v-on="on"
                      solo
                      placeholder="Dirección de Destino"
                      Label="Dirección de Destino"
                      color="red"
                      v-model="destination.destination"
                    >
                      <template v-slot:prepend-inner>
                        <v-icon v-on="on" color="red"> mdi-map-marker</v-icon>
                      </template>
                      <template v-slot:append>
                        <v-icon v-on="on" color="red"> mdi-pencil</v-icon>
                      </template>
                    </v-text-field>
                  </template>
                  <br />
                  <v-card>
                    <v-toolbar dark color="black">
                      <v-btn icon dark @click="destination.dialog = false">
                        <v-icon>mdi-keyboard-backspace</v-icon>
                      </v-btn>
                      <v-toolbar-title>Destino</v-toolbar-title>
                      <v-spacer></v-spacer>
                      <v-toolbar-items>
                        <v-btn dark text @click="handleSaveDestination()">
                          Guardar
                        </v-btn>
                      </v-toolbar-items>
                    </v-toolbar>
                    <v-list three-line subheader>
                      <v-subheader>Detalles de Destino</v-subheader>
                      <v-list-item>
                        <v-list-item-content>
                          <v-list-item-title
                            >Especificar detalles de Destino</v-list-item-title
                          >
                          <v-list-item-subtitle
                            >Mensaje relacionado a detalles de destino <br />
                            <span class="text-decoration-underline"
                              >Más Información</span
                            >
                            <v-icon
                              >mdi-information-outline</v-icon
                            ></v-list-item-subtitle
                          >
                        </v-list-item-content>
                      </v-list-item>
                    </v-list>
                    <v-divider></v-divider>
                    <v-container>
                      <v-row dense>
                        <v-col>
                          <v-img
                            src="@/assets/map.png"
                            alt="mapa"
                            max-height="400"
                            max-width="100%"
                          ></v-img>
                        </v-col>
                      </v-row>
                      <v-row dense>
                        <v-col>
                          <v-text-field
                            solo
                            placeholder="Dirección de Destino"
                            label="Dirección de Destino"
                            prepend-inner-icon="mdi-map-marker"
                            color="red"
                            v-model="destination.destination"
                          >
                            <template v-slot:prepend-inner>
                              <v-icon v-on="on" color="red">
                                mdi-map-marker</v-icon
                              >
                            </template>
                          </v-text-field>
                        </v-col>
                      </v-row>
                      <v-row dense>
                        <v-col>
                          <label>Detalles de Destino</label>
                          <v-textarea
                            solo
                            placeholder="Entregar a Pedro en sus manos"
                            v-model="destination.details"
                          ></v-textarea>
                        </v-col>
                      </v-row>
                      <v-row dense>
                        <v-col>
                          <v-radio-group
                            v-model="destination.radioGroup"
                            class="d-flex flex-row"
                            label="Quién entrega el paquete?"
                          >
                            <v-radio label="Yo" value="Yo"></v-radio>
                            <v-radio label="Tercero" value="Tercero"></v-radio>
                          </v-radio-group>
                        </v-col>
                      </v-row>
                      <v-row dense v-if="destination.radioGroup === 'Tercero'">
                        <v-col>
                          <v-text-field
                            v-model="destination.thirdName"
                            label="Nombre de Tercero"
                            prepend-icon="mdi-account"
                          />
                        </v-col>
                        <v-col>
                          <v-text-field
                            v-model="destination.thirdPhone"
                            label="Número de Tercero"
                            type="number"
                            prepend-icon="mdi-cellphone"
                          />
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card>
                </v-dialog>
              </v-col>
            </v-row>
            <v-row dense v-if="destination.destination">
              <v-col>
                <v-expansion-panels v-model="panel" :disabled="disabled">
                  <v-expansion-panel>
                    <v-expansion-panel-header
                      >Detalle de Destino</v-expansion-panel-header
                    >
                    <v-expansion-panel-content>
                      <ul>
                        <li>Detalles: {{ destination.details }}</li>
                        <li>
                          Persona a entregar: {{ destination.radioGroup }}
                        </li>
                        <span v-if="destination.radioGroup === 'Tercero'">
                          <li>
                            Número de Tercero: {{ destination.thirdPhone }}
                          </li>
                          <li>
                            Nombre de Tercero: {{ destination.thirdName }}
                          </li>
                        </span>
                      </ul>
                    </v-expansion-panel-content>
                  </v-expansion-panel>
                </v-expansion-panels>
              </v-col>
            </v-row>
          </v-card>
        </v-col>
      </v-row>

      <!-- Paquete -->
      <v-row>
        <v-col>
          <v-card class="pa-4">
            <v-row dense>
              <v-col>
                <h2 class="font-weight-bold">
                  Detalles del Paquete
                  <v-icon color="black">mdi-package-variant-closed</v-icon>
                </h2>
              </v-col>
            </v-row>
            <v-row>
              <v-col class="text-center">
                <v-dialog
                  v-model="paquete.dialog"
                  fullscreen
                  hide-overlay
                  transition="dialog-bottom-transition"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn
                      large
                      class="mx-2"
                      fab
                      dark
                      small
                      color="black"
                      v-bind="attrs"
                      v-on="on"
                    >
                      <v-icon dark> mdi-pencil </v-icon>
                    </v-btn>
                  </template>
                  <v-card>
                    <v-toolbar dark color="black">
                      <v-btn icon dark @click="paquete.dialog = false">
                        <v-icon>mdi-keyboard-backspace</v-icon>
                      </v-btn>
                      <v-toolbar-title>Detalles de Paquete</v-toolbar-title>
                      <v-spacer></v-spacer>
                      <v-toolbar-items>
                        <v-btn dark text @click="handleSavePaquete()">
                          Guardar
                        </v-btn>
                      </v-toolbar-items>
                    </v-toolbar>
                    <v-list three-line subheader>
                      <v-subheader>Detalles de Paquete</v-subheader>
                      <v-list-item>
                        <v-list-item-content>
                          <v-list-item-title
                            >Especificar detalles de Paquete</v-list-item-title
                          >
                          <v-list-item-subtitle
                            >Mensaje relacionado a detalles de paquete <br />
                            <span class="text-decoration-underline"
                              >Más Información</span
                            >
                            <v-icon
                              >mdi-information-outline</v-icon
                            ></v-list-item-subtitle
                          >
                        </v-list-item-content>
                      </v-list-item>
                    </v-list>
                    <v-divider></v-divider>
                    <v-container>
                      <v-row dense>
                        <v-col>
                          <label>Volumen del Paquete</label>
                          <v-select
                            solo
                            label="Volumen"
                            :items="paquete.volumesList"
                            v-model="paquete.volume"
                          ></v-select>
                        </v-col>
                      </v-row>
                      <v-row dense>
                        <v-col>
                          <label>Peso del Paquete (Kg)</label>
                          <v-autocomplete
                            solo
                            label="Peso del Paquete"
                            :items="paquete.massList"
                            v-model="paquete.mass"
                          ></v-autocomplete>
                        </v-col>
                      </v-row>
                      <v-row dense>
                        <v-col>
                          <label>Tipo de Paquete</label>
                          <v-autocomplete
                            solo
                            label="Tipo de Paquete"
                            :items="paquete.typeList"
                            v-model="paquete.type"
                          ></v-autocomplete>
                        </v-col>
                      </v-row>
                      <v-row dense>
                        <v-col>
                          <label>Descripción de Paquete</label>
                          <v-textarea
                            solo
                            placeholder="Franela tal..."
                            v-model="paquete.details"
                          ></v-textarea>
                        </v-col>
                      </v-row>
                      <v-row dense>
                        <v-col>
                          <label>Adjuntar fotografía</label>
                          <v-file-input
                            accept="image/png, image/jpeg, image/bmp"
                            placeholder="Adjuntar fotografía"
                            prepend-icon="mdi-camera"
                            label="Adjuntar fotografía"
                            color="cyan"
                            solo
                          >
                          </v-file-input>
                        </v-col>
                      </v-row>
                      <v-row dense>
                        <v-col>
                          <v-radio-group
                            v-model="paquete.secure"
                            class="d-flex flex-row"
                            label="Asegurar Paquete?"
                          >
                            <v-radio label="No" :value="false"></v-radio>
                            <v-radio label="Sí" :value="true"></v-radio>
                          </v-radio-group>
                        </v-col>
                      </v-row>
                      <v-row dense v-if="paquete.secure">
                        <v-col>
                          <v-select
                            v-model="paquete.secureType"
                            label="Tipo de Seguro"
                            prepend-icon="mdi-lock"
                            :items="paquete.secureTypeList"
                          />
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card>
                </v-dialog>
              </v-col>
            </v-row>
            <span v-if="paquete.mass">
              <br />
              <br />
            </span>
            <v-row dense v-if="paquete.mass">
              <v-col>
                <v-expansion-panels>
                  <v-expansion-panel>
                    <v-expansion-panel-header
                      >Detalles del Paquete</v-expansion-panel-header
                    >
                    <v-expansion-panel-content>
                      <ul>
                        <li>Volumen: {{ paquete.volumne }}</li>
                        <li>Peso: {{ paquete.mass }}</li>
                        <li>Tipo: {{ paquete.type }}</li>
                        <li>Detalles: {{ paquete.details }}</li>
                        <li>Asegurado?: {{ paquete.secure }}</li>
                        <li v-if="paquete.secure">
                          Tipo de Seguro: {{ paquete.secureType }}
                        </li>
                      </ul>
                    </v-expansion-panel-content>
                  </v-expansion-panel>
                </v-expansion-panels>
              </v-col>
            </v-row>
          </v-card>
        </v-col>
      </v-row>

      <!-- Agendar -->
      <v-row>
        <v-col>
          <v-card class="pa-4">
            <v-row dense>
              <v-col>
                <h2 class="font-weight-bold">
                  Detalles de Hora de Pickup
                  <v-icon color="black">mdi-clock</v-icon>
                </h2>
              </v-col>
            </v-row>
            <v-row dense>
              <v-col>
                <v-radio-group
                  v-model="schedule.schedule"
                  class="d-flex flex-row"
                >
                  <v-radio
                    :value="false"
                    label="Instantáneo (30 minuros o menos)"
                    color="black"
                  ></v-radio>
                  <v-radio
                    :value="true"
                    label="Agendado"
                    color="black"
                  ></v-radio>
                </v-radio-group>
              </v-col>
            </v-row>
            <v-row dense v-if="schedule.schedule">
              <v-col>
                <v-datetime-picker
                  label="Seleccione Día y Hora"
                  v-model="schedule.time"
                >
                </v-datetime-picker>
              </v-col>
            </v-row>
          </v-card>
        </v-col>
      </v-row>

      <!-- Flota -->
      <v-row>
        <v-col>
          <v-card class="pa-4">
            <v-row dense>
              <v-col>
                <h2 class="font-weight-bold">
                  Detalles de Flota
                  <v-icon color="black">mdi-car</v-icon>
                </h2>
              </v-col>
            </v-row>
            <v-row dense>
              <v-col>
                <v-slide-group v-model="fleet.fleet" class="pa-4">
                  <v-slide-item
                    v-for="fleet in fleet.fleetList"
                    :key="fleet.index"
                    v-slot="{ active, toggle }"
                  >
                    <v-card
                      :color="active ? 'primary' : 'grey lighten-1'"
                      class="ma-4 white--text font-weight-bold"
                      height="200"
                      width="200"
                      @click="toggle"
                    >
                      <v-row
                        class="fill-height"
                        align="center"
                        justify="center"
                      >
                        <v-scale-transition>
                          <v-icon
                            v-if="active"
                            color="white"
                            size="48"
                          ></v-icon>
                        </v-scale-transition>
                        <span
                          class="d-flex justify-center flex-column text-center"
                        >
                          <span style="min-height: 10px">
                            <v-icon x-large color="white">{{
                              fleet.icon
                            }}</v-icon>
                          </span>
                          <br />
                          {{ fleet.name }}
                          <br />
                          <p>Precio XX</p>
                        </span>
                      </v-row>
                    </v-card>
                  </v-slide-item>
                </v-slide-group>
              </v-col>
            </v-row>
          </v-card>
        </v-col>
      </v-row>

      <!-- siguiente -->
      <v-row>
        <v-col>
          <v-btn block color="black" dark @click="step++"> Siguiente </v-btn>
        </v-col>
      </v-row>
    </span>

    <!-- step 1 -->
    <span v-if="step === 1">
      <v-row>
        <v-col>
          <v-expansion-panels>
            <v-expansion-panel>
              <v-expansion-panel-header
                >Resumen de Envío</v-expansion-panel-header
              >
              <v-expansion-panel-content>
                <v-card class="pa-4">
                  <v-row dense>
                    <v-col class="d-flex flex-row justify-space-between">
                      <h2 class="font-weight-bold">
                        Resumen del Envío
                        <v-icon color="black">mdi-truck</v-icon>
                      </h2>
                      <v-icon color="black"> mdi-share-variant-outline </v-icon>
                    </v-col>
                  </v-row>
                  <v-row dense>
                    <v-col>
                      <h3>Coordenadas de Envío</h3>
                    </v-col>
                  </v-row>
                  <v-row dense>
                    <v-col>
                      <li>
                        <span class="font-weight-bold">
                          Origen: {{ origin.origin }}
                        </span>
                        <ul>
                          <li>Detalles: {{ origin.details }}</li>
                          <li>Persona a Entregar: {{ origin.radioGroup }}</li>
                          <ul v-if="origin.radioGroup === 'Tercero'">
                            <li>Nombre de Tercero: {{ origin.thirdPhone }}</li>
                            <li>Número de Tercero: {{ origin.thirdName }}</li>
                          </ul>
                        </ul>
                      </li>
                    </v-col>
                  </v-row>
                  <v-row dense>
                    <v-col>
                      <li>
                        <span class="font-weight-bold">
                          Destino: {{ destination.destination }}
                        </span>
                        <ul>
                          <li>Detalles: {{ destination.details }}</li>
                          <li>
                            Persona a Entregar: {{ destination.radioGroup }}
                          </li>
                          <ul v-if="destination.radioGroup === 'Tercero'">
                            <li>
                              Nombre de Tercero: {{ destination.thirdPhone }}
                            </li>
                            <li>
                              Número de Tercero: {{ destination.thirdName }}
                            </li>
                          </ul>
                        </ul>
                      </li>
                    </v-col>
                  </v-row>
                  <v-row dense>
                    <v-col>
                      <h3>Detalles de Paquete</h3>
                    </v-col>
                  </v-row>
                  <v-row dense>
                    <v-col>
                      <li>Volumen: {{ paquete.volume }}</li>
                      <li>Masa: {{ paquete.mass }}</li>
                      <li>Tipo: {{ paquete.type }}</li>
                      <li>Detalles: {{ paquete.details }}</li>
                      <li>
                        Asegurado ?
                        <ul>
                          <li>
                            {{ paquete.secure ? "Sí" : "No" }}
                          </li>
                          <li v-if="paquete.secure">
                            Tipo: {{ paquete.secureType }}
                          </li>
                        </ul>
                      </li>
                    </v-col>
                  </v-row>
                  <v-row dense>
                    <v-col>
                      <h3>Detalles de Pick Up Time</h3>
                    </v-col>
                  </v-row>
                  <v-row dense>
                    <ul>
                      <li>
                        {{ schedule.schedule ? "Agendado" : "Instantáneo" }}
                      </li>
                      <ul v-if="schedule.schedule">
                        <li>Día y Hora{{ schedule.time }}</li>
                      </ul>
                    </ul>
                  </v-row>
                  <v-row dense>
                    <v-col>
                      <h3>Detalles de Flota</h3>
                    </v-col>
                  </v-row>
                  <v-row dense>
                    <v-col>
                      <ul>
                        <li>
                          <v-icon color="black">{{ handleFleet.icon }}</v-icon>
                          - {{ handleFleet.name }}
                        </li>
                      </ul>
                    </v-col>
                  </v-row>
                </v-card>
              </v-expansion-panel-content>
            </v-expansion-panel>
          </v-expansion-panels>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-card class="pa-4">
            <v-row>
              <v-col>
                <h2 class="font-weight-bold">
                  Pagar
                  <v-icon color="black">mdi-cash-register</v-icon>
                </h2>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <p>Envío Flota {{ handleFleet.name }}: $xx</p>
                <p v-if="paquete.secure">
                  Seguro: {{ paquete.secureType }} - $xx
                </p>
                <p>
                  <v-text-field
                    solo
                    label="PromoCode"
                    placeholder="Coloque PromoCode"
                    append-icon="mdi-send"
                    color="black"
                  >
                  </v-text-field>
                </p>
                <h3 class="cyan--text">Total envío: $XX</h3>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <label>Método de Pago</label>
                <v-select
                  solo
                  :items="payment.paymentList"
                  v-model="payment.payment"
                >
                </v-select>
              </v-col>
            </v-row>
          </v-card>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-card class="pa-4">
            <v-row dense>
              <v-col>
                <v-checkbox v-model="checkbox.checkbox">
                  <template v-slot:label>
                    <div>
                      Al aceptar el servicio usted acepta los
                      <span class="text-decoration-underline"
                        >Términos y Condiciones</span
                      >
                    </div>
                  </template>
                </v-checkbox>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-btn  :disabled="!checkbox.checkbox" block color="primary" @click="handleEncomienda()">
                  Encomienda
                </v-btn>
              </v-col>
            </v-row>
          </v-card>
        </v-col>
      </v-row>
    </span>

    <!-- snackbar -->
    <v-snackbar v-model="snackbar.snackbar" :timeout="snackbar.timeout">
      {{ snackbar.text }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="blue"
          text
          v-bind="attrs"
          @click="snackbar.snackbar = false"
        >
          Cerrar
        </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>

<script>
export default {
  name: "Mandadito",

  data: () => ({
    snackbar: {
      snackbar: false,
      text: "Guardado",
      timeout: 2000,
    },
    origin: {
      dialog: false,
      origin: "",
      details: "details origin",
      radioGroup: "Yo",
      thirdPhone: "123456",
      thirdName: "Third Person",
    },
    destination: {
      dialog: false,
      destination: "",
      details: "details destination",
      radioGroup: "Tercero",
      thirdPhone: "123456",
      thirdName: "Third Person",
    },
    paquete: {
      dialog: false,
      volumesList: [" < peq - moto", " < med - carro", " < gran - camioneta"],
      volume: " < peq - moto",
      massList: [
        1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20,
        21, 22, 23, 24, 25, 26, 27, 28, 29, 30,
      ],
      mass: "",
      typeList: [
        "Documento",
        "Ropa",
        "Comida",
        "Frágil",
        "Medicina",
        "Electrodoméstico",
        "Artículo Deportivo",
      ],
      type: "",
      details: "",
      secure: false,
      secureTypeList: ["básico $", "medio $$", "grande $$$"],
      secureType: "",
    },
    schedule: {
      schedule: false,
      time: "",
    },
    fleet: {
      fleetList: [
        {
          name: "Bicicleta",
          icon: "mdi-bicycle",
        },
        {
          name: "Moto",
          icon: "mdi-motorbike",
        },
        {
          name: "Carro",
          icon: "mdi-car",
        },
        {
          name: "Camioneta",
          icon: "mdi-car-estate",
        },
      ],
      fleet: 0,
    },
    step: 1,
    payment: {
      paymentList: ["Wallet", "Zelle", "tdc", "pago móvil automatizado"],
      payment: "Wallet",
    },
    checkbox: {
      checkbox: false,
    },
  }),
  methods: {
    handleSaveOrigin() {
      this.origin.dialog = false;
      this.snackbar.snackbar = true;
    },
    handleSaveDestination() {
      this.destination.dialog = false;
      this.snackbar.snackbar = true;
    },
    handleSavePaquete() {
      this.paquete.dialog = false;
      this.snackbar.snackbar = true;
    },
    handleEncomienda() {
      alert('Encomienda enviada a Pool')
      this.step = 0
    }
  },
  computed: {
    handleFleet() {
      let return_ = "";
      switch (this.fleet.fleet) {
        case 0:
          return_ = {
            name: "Bicicleta",
            icon: "mdi-bicycle",
          };
          break;
        case 1:
          return_ = {
            name: "Moto",
            icon: "mdi-motorbike",
          };
          break;

        case 2:
          return_ = {
            name: "Carro",
            icon: "mdi-car",
          };
          break;

        case 3:
          return_ = {
            name: "Camioneta",
            icon: "mdi-car-estate",
          };
          break;
        default:
          return_ = {
            name: "Bicicleta",
            icon: "mdi-bicycle",
          };
          break;
      }
      return return_;
    },
  },
};
</script>

<style lang="sass"></style>
