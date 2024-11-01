<template>
  <div class="parentContainer">
    <div class="container">



      <div class="searchContainer">
        <v-text-field v-model="search" label="Search" prepend-inner-icon="mdi-magnify" variant="outlined" hide-details
          single-line class="search"></v-text-field>
      </div>


      <v-data-table
    v-model:expanded="expanded"
    :headers="headers"
    :items="filteredUsers"
    :search="search"
    class="table"
    show-expand
    style="text-align: center; width: 99%; margin: 0 auto; min-height: 77vh; max-height: 77vh;"
  >
    <!-- Toolbar -->
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>User Management</v-toolbar-title>
      </v-toolbar>
    </template>

    <!-- Expanded Row with Advanced Layout -->
    <template v-slot:expanded-row="{ columns, item }">
      <tr>
        <td :colspan="columns.length">
          <v-card class="expanded-row-card" elevation="2">
            <v-container>
              <v-row>
                <!-- User Avatar and Main Info -->
                <v-col cols="3" class="text-center">
                  <v-avatar size="80">
                    <v-img :src="item.profilePicture || 'https://via.placeholder.com/80x80'" />
                  </v-avatar>
                  <p><strong>{{ item.fullname }}</strong></p>
                  <v-chip :color="item.isactive ? 'green' : 'red'" dark>
                    {{ item.isactive ? 'Active' : 'Inactive' }}
                  </v-chip>
                </v-col>

                <!-- Tabbed Interface for Info -->
                <v-col cols="9">
                  <v-tabs v-model="activeTab">
                    <v-tab>Personal Info</v-tab>
                    <v-tab>Work Info</v-tab>
                    <v-tab>Status</v-tab>
                  </v-tabs>

                  <v-tabs-items v-model="activeTab">
                    <!-- Personal Info Tab -->
                    <v-tab-item value="0">
                      <v-row>
                        <v-col cols="6">
                          <p><strong>Matricule:</strong> {{ item.matricule }}</p>
                          <p><strong>First Name:</strong> {{ item.firstname }}</p>
                        </v-col>
                        <v-col cols="6">
                          <p><strong>Last Name:</strong> {{ item.lastname }}</p>
                          <p><strong>Email:</strong> {{ item.email }}</p>
                        </v-col>
                      </v-row>
                    </v-tab-item>

                    <!-- Work Info Tab -->
                    <v-tab-item value="1">
                      <v-row>
                        <v-col cols="6">
                          <p><strong>Role:</strong>
                            <v-chip color="blue" outlined>
                              <v-icon left>mdi-account</v-icon> {{ item.roleName }}
                            </v-chip>
                          </p>
                          <p><strong>Shift:</strong> {{ item.shiftName }}</p>
                        </v-col>
                        <v-col cols="6">
                          <p><strong>Profile Group:</strong> {{ item.profile_groupName }}</p>
                          <p><strong>Working Hours:</strong> {{ item.workingHours }}</p>
                        </v-col>
                      </v-row>
                    </v-tab-item>

                    <!-- Status Tab -->
                    <v-tab-item value="2">
                      <v-row>
                        <v-col cols="6">
                          <p><strong>Global WH:</strong> {{ item.wh_global }}</p>
                          <p><strong>Standby Hours:</strong> {{ item.sby_workingHours }}</p>
                        </v-col>
                        <v-col cols="6">
                          <v-chip color="green" text-color="white" v-if="item.isactive">
                            Account Enabled
                          </v-chip>
                          <v-chip color="red" text-color="white" v-else>
                            Account Disabled
                          </v-chip>
                        </v-col>
                      </v-row>
                    </v-tab-item>
                  </v-tabs-items>
                </v-col>
              </v-row>

              <!-- Action Buttons with Tooltips -->
              <v-row class="justify-end action-buttons-row">
                <v-tooltip bottom>
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn icon v-bind="attrs" v-on="on" color="blue darken-2" @click="editItem(item)">
                      <v-icon>mdi-pencil</v-icon>
                    </v-btn>
                  </template>
                  <span>Edit User</span>
                </v-tooltip>

                <v-tooltip bottom>
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn icon v-bind="attrs" v-on="on" color="red darken-2" @click="deleteItem(item)">
                      <v-icon>mdi-delete</v-icon>
                    </v-btn>
                  </template>
                  <span>Delete User</span>
                </v-tooltip>

                <v-tooltip bottom>
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn icon v-bind="attrs" v-on="on" color="orange darken-3" @click="resetItem(item)">
                      <v-icon>mdi-rotate-left</v-icon>
                    </v-btn>
                  </template>
                  <span>Reset Password</span>
                </v-tooltip>
              </v-row>
            </v-container>
          </v-card>
        </td>
      </tr>
    </template>
  </v-data-table>

      <v-data-table v-if="false" v-model:expanded="expanded" :headers="headers" :items="filteredUsers" :search="search"
        class="table" show-expand
        style="text-align: center; width: 99%; margin:0 auto; min-height: 77vh; max-height: 77vh;">
        <!-- Toolbar -->
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>User Management</v-toolbar-title>
          </v-toolbar>
        </template>

        <!-- Expanded Row Structure -->
        <template v-slot:expanded-row="{ columns, item }">
          <tr>
            <td :colspan="columns.length">
              <v-card class="expanded-row-card" outlined>
                <v-container>
                  <!-- Personal Info Section -->
                  <v-row>
                    <v-col cols="4">
                      <h5 class="section-title">Personal Information</h5>
                      <p><strong>Matricule:</strong> {{ item.matricule }}</p>
                      <p><strong>First Name:</strong> {{ item.firstname }}</p>
                      <p><strong>Last Name:</strong> {{ item.lastname }}</p>
                      <p><strong>Email:</strong> {{ item.email }}</p>
                    </v-col>

                    <!-- Work Info Section -->
                    <v-col cols="4">
                      <h5 class="section-title">Work Information</h5>
                      <p><strong>Role:</strong> {{ item.roleName }}</p>
                      <p><strong>Shift:</strong> {{ item.shiftName }}</p>
                      <p><strong>Profile Group:</strong> {{ item.profile_groupName }}</p>
                      <p><strong>Working Hours:</strong> {{ item.workingHours }}</p>
                    </v-col>

                    <!-- Extra Info Section -->
                    <v-col cols="4">
                      <h5 class="section-title">Additional Info</h5>
                      <p><strong>Global WH:</strong> {{ item.wh_global }}</p>
                      <p><strong>Standby Hours:</strong> {{ item.sby_workingHours }}</p>
                      <p><strong>Status:</strong> {{ item.isactive ? 'Active' : 'Inactive' }}</p>
                    </v-col>
                  </v-row>

                  <!-- Action Buttons -->
                  <v-row class="justify-end action-buttons-row">
                    <v-btn icon color="green-darken-4" @click="editItem(item)">
                      <v-icon>mdi-pencil</v-icon>
                    </v-btn>

                    <v-btn icon color="red" @click="deleteItem(item)">
                      <v-icon>mdi-delete</v-icon>
                    </v-btn>

                    <v-btn icon color="yellow-darken-4" @click="resetItem(item)">
                      <v-icon>mdi-rotate-left</v-icon>
                    </v-btn>
                  </v-row>
                </v-container>
              </v-card>
            </td>
          </tr>
        </template>
      </v-data-table>


      <v-data-table v-if="false" v-model:expanded="expanded" :headers="headers" :items="filteredUsers" :search="search"
        class="table" show-expand style=" text-align: center; width: 99%; margin:0 auto;    min-height: 77vh;
    max-height: 77vh;">
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>Expandable Table</v-toolbar-title>
          </v-toolbar>
        </template>

        <template v-slot:expanded-row="{ columns, item }">
          <tr>
            <td :colspan="columns.length">
              More info about {{ item.matricule }}
            </td>
          </tr>
        </template>
      </v-data-table>


      <v-data-table v-if="false" :headers="headers" :items="filteredUsers" :search="search" class="table" style=" text-align: center; width: 80%; margin:0 auto;    min-height: 77vh;
    max-height: 77vh;">
        <!-- Add new user part -->
        <template v-slot:top>
          <v-toolbar flat v-bind:class="{ 'addUserButtonZone': true }">
            <!-- Dialog to add new user -->
            <v-dialog v-model="dialog" max-width="500px">
              <!-- Button to display add user dialog -->
              <template v-slot:activator="{ props }">
                <div class="addActions">
                  <div>
                    <v-btn style="background-color: #009688;" icon color="white" @click="resetWHShowfunction"
                      v-bind:class="{ 'importUsersButton': true }">
                      <v-icon size="small">
                        mdi-clipboard-clock-outline
                      </v-icon>
                      <input type="file" ref="fileInput" style="display: none" accept=".xls,.xlsx"
                        @change="handleFileUpload">
                    </v-btn>
                  </div>
                  <div>
                    <v-btn icon color="white" @click="openFileDialog" v-bind:class="{ 'importUsersButton': true }">
                      <v-icon size="small">
                        mdi-upload
                      </v-icon>
                      <input type="file" ref="fileInput" style="display: none" accept=".xls,.xlsx"
                        @change="handleFileUpload">
                    </v-btn>
                  </div>
                  <div class="addUserButtonStyle">
                    <v-btn icon v-bind="props" color="white" v-bind:class="{ 'addUserButton': true }">
                      <v-icon size="small">
                        mdi-account-plus-outline
                      </v-icon>
                    </v-btn>
                  </div>
                </div>

              </template>
              <!-- Add dialog content -->
              <v-card class="dialog">
                <div class="dialogTop">
                  <div>
                    <v-card-title class="dialogAddUserTitleContainer">
                      <span class="text-h5">{{ dialogType === 'edit' ? 'Edit user' : 'New user'
                        }}</span>
                    </v-card-title>
                  </div>

                </div>
                <v-form @submit.prevent="save">
                  <v-card-text>
                    <v-container>
                      <v-row cols="16">
                        <v-col cols="16">
                          <v-text-field v-model="user.matricule" label="Matricule" class="custom-text-field"
                            :rules="[submitClicked ? requiredRule(user.matricule, 'matricule') : () => true]"></v-text-field>
                        </v-col>
                      </v-row>

                      <v-row cols="16">
                        <v-col cols="16">
                          <v-text-field v-model="user.firstname" label="Firstname" class="custom-text-field"
                            :rules="[submitClicked ? requiredRule(user.firstname, 'firstname') : () => true]"></v-text-field>
                        </v-col>
                      </v-row>

                      <v-row cols="16">
                        <v-col cols="16">
                          <v-text-field v-model="user.lastname" label="Lastname" class="custom-text-field"
                            :rules="[submitClicked ? requiredRule(user.lastname, 'lastname') : () => true]"></v-text-field>
                        </v-col>
                      </v-row>

                      <v-row cols="16">
                        <v-col cols="16">
                          <v-text-field v-model="user.email" label="Email"
                            :rules="[submitClicked ? requiredRule(user.email, 'email') : () => true]"></v-text-field>
                        </v-col>
                      </v-row>

                      <v-row cols="16">
                        <v-col cols="16">
                          <v-select :items="roles" density="default" item-title="name" item-value="id" label="Roles"
                            v-model="user.role_id"
                            :rules="[submitClicked ? requiredRule(user.role_id, 'role') : () => true]">
                            ></v-select>
                        </v-col>
                      </v-row>
                      <v-row cols="16">
                        <v-col cols="16">
                          <v-select :items="shifts" density="default" item-title="category" item-value="id"
                            label="Shifts" v-model="user.shift_id"
                            :rules="[submitClicked ? requiredRule(user.shift_id, 'shift') : () => true]">
                            ></v-select>
                        </v-col>
                      </v-row>
                      <v-row cols="16">
                        <v-col cols="16">
                          <v-select :items="profileGroups" density="default" item-title="type" item-value="id"
                            label="Profile Groups" v-model="user.profile_group_id"
                            :rules="[submitClicked ? requiredRule(user.profile_group_id, 'profile_group') : () => true]">
                            ></v-select>
                        </v-col>
                      </v-row>

                      <v-row cols="16" v-if="dialogType === 'edit'">
                        <v-col cols="16">
                          <div>
                            <label for="radio-group" style="font-weight: bold;">Account:</label>
                            <v-radio-group v-model="user.isactive" id="radio-group" inline>
                              <v-radio label="Enable" value=1></v-radio>
                              <v-radio label="Disable" value=0></v-radio>
                            </v-radio-group>
                          </div>
                        </v-col>
                      </v-row>
                    </v-container>

                  </v-card-text>
                  <!-- Actions to save or cancel -->
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="white" class="dialogCancel" variant="text" @click="close">
                      Cancel
                    </v-btn>
                    <v-btn color="white" variant="text" class="dialogOk" type="submit">
                      Save
                    </v-btn>
                  </v-card-actions>
                </v-form>
              </v-card>
            </v-dialog>
            <!-- Dialog for delete confirm-->
            <v-dialog v-model="dialogDelete" max-width="500px">
              <v-card>
                <v-card-title class="dialogText">Are you sure you want to delete this
                  user?</v-card-title>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn class="dialogCancel" variant="text" @click="closeDelete">Cancel</v-btn>
                  <v-btn class="dialogOk" variant="text" @click="deleteItemConfirm">OK</v-btn>
                  <v-spacer></v-spacer>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <!-- Dialog for reseting passsword -->
            <v-dialog v-model="dialogReset" max-width="500px">
              <v-card>
                <v-card-title class="dialogText">Are you sure you want to reset password for this
                  user?</v-card-title>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn class="dialogCancel" variant="text" @click="closeReset">Cancel</v-btn>
                  <v-btn class="dialogOk" variant="text" @click="resetPasswordConfirm">OK</v-btn>
                  <v-spacer></v-spacer>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-toolbar>
        </template>

        <template v-slot:item="{ item }">
          <tr :class="{ 'inactive-user': item.isactive === 0 }">
            <td>{{ item.fullname }}</td>
            <td>{{ item.matricule }}</td>
            <td>{{ item.email }}</td>
            <td>{{ item.roleName }}</td>
            <td>{{ item.profile_groupName }}</td>
            <td>{{ item.shiftName }}</td>
            <td>{{ item.workingHours }}</td>
            <td>{{ item.wh_global }}</td>
            <td>{{ item.sby_workingHours }}</td>
            <td>
              <div class="actions">
                <v-btn icon class="icon" size="small" color="green-darken-4" @click="editItem(item)">
                  <v-icon>
                    mdi-pencil
                  </v-icon>
                </v-btn>

                <v-btn icon class="icon" size="small" color="red" @click="deleteItem(item)">
                  <v-icon>
                    mdi-delete
                  </v-icon>
                </v-btn icon>

                <v-btn icon class="icon" size="small" color="yellow-darken-4" @click="resetItem(item)">
                  <v-icon>
                    mdi-rotate-left
                  </v-icon>
                </v-btn icon>
              </div>
            </td>
          </tr>
        </template>
      </v-data-table>
    </div>
  </div>
  <template>
    <div class="text-center pa-4">
      <v-dialog v-model="resetWHShow" max-width="1000" persistent>
        <v-card class="confirmDialogInterface" prepend-icon="mdi-clipboard-clock-outline"
          text="Please select the teams to reset their WH:" title="WORKING HOURS">
          <v-card-text class="mb-0 pb-0">
            <div class="table-container">
              <table>
                <thead>
                  <tr>
                    <th>SHIFT</th>
                    <th>RTG</th>
                    <th>STS</th>
                    <th>RS</th>
                    <th>AM</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <th>A</th>
                    <td class="shiftWHselect" @click="whResetCatalogueFunction(1, 1, 'RTG', 'A')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(1, 1).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(1, 1).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(1, 1).last.created_at ? find_WH_reset_row(1, 1).last.created_at : 'empty'
                        }}</span>
                    </td>


                    <td class="shiftWHselect" @click="whResetCatalogueFunction(2, 1, 'STS', 'A')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(2, 1).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(2, 1).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(2, 1).last.created_at ? find_WH_reset_row(2, 1).last.created_at : 'empty'
                        }}</span>
                    </td>


                    <td class="shiftWHselect" @click="whResetCatalogueFunction(3, 1, 'RS', 'A')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(3, 1).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(3, 1).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(3, 1).last.created_at ? find_WH_reset_row(3, 1).last.created_at : 'empty'
                        }}</span>
                    </td>


                    <td class="shiftWHselect" @click="whResetCatalogueFunction(4, 1, 'AM', 'A')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(4, 1).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(4, 1).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(4, 1).last.created_at ? find_WH_reset_row(4, 1).last.created_at : 'empty'
                        }}</span>
                    </td>


                  </tr>
                  <tr>
                    <th>B</th>
                    <td class="shiftWHselect" @click="whResetCatalogueFunction(1, 2, 'RTG', 'B')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(1, 2).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(1, 2).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(1, 2).last.created_at ? find_WH_reset_row(1, 2).last.created_at : 'empty'
                        }}</span>
                    </td>


                    <td class="shiftWHselect" @click="whResetCatalogueFunction(2, 2, 'STS', 'B')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(2, 2).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(2, 2).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(2, 2).last.created_at ? find_WH_reset_row(2, 2).last.created_at : 'empty'
                        }}</span>
                    </td>


                    <td class="shiftWHselect" @click="whResetCatalogueFunction(3, 2, 'RS', 'B')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(3, 2).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(3, 2).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(3, 2).last.created_at ? find_WH_reset_row(3, 2).last.created_at : 'empty'
                        }}</span>
                    </td>


                    <td class="shiftWHselect" @click="whResetCatalogueFunction(4, 2, 'AM', 'B')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(4, 2).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(4, 2).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(4, 2).last.created_at ? find_WH_reset_row(4, 2).last.created_at : 'empty'
                        }}</span>
                    </td>


                  </tr>
                  <tr>
                    <th>C</th>
                    <td class="shiftWHselect" @click="whResetCatalogueFunction(1, 3, 'RTG', 'C')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(1, 3).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(1, 3).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(1, 3).last.created_at ? find_WH_reset_row(1, 3).last.created_at : 'empty'
                        }}</span>
                    </td>


                    <td class="shiftWHselect" @click="whResetCatalogueFunction(2, 3, 'STS', 'C')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(2, 3).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(2, 3).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(2, 3).last.created_at ? find_WH_reset_row(2, 3).last.created_at : 'empty'
                        }}</span>
                    </td>


                    <td class="shiftWHselect" @click="whResetCatalogueFunction(3, 3, 'RS', 'C')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(3, 3).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(3, 3).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(3, 3).last.created_at ? find_WH_reset_row(3, 3).last.created_at : 'empty'
                        }}</span>
                    </td>


                    <td class="shiftWHselect" @click="whResetCatalogueFunction(4, 3, 'AM', 'C')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(4, 3).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(4, 3).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(4, 3).last.created_at ? find_WH_reset_row(4, 3).last.created_at : 'empty'
                        }}</span>
                    </td>


                  </tr>
                  <tr>
                    <th>D</th>
                    <td class="shiftWHselect" @click="whResetCatalogueFunction(1, 4, 'RTG', 'D')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(1, 4).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(1, 4).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(1, 4).last.created_at ? find_WH_reset_row(1, 4).last.created_at : 'empty'
                        }}</span>
                    </td>


                    <td class="shiftWHselect" @click="whResetCatalogueFunction(2, 4, 'STS', 'D')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(2, 4).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(2, 4).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(2, 4).last.created_at ? find_WH_reset_row(2, 4).last.created_at : 'empty'
                        }}</span>
                    </td>


                    <td class="shiftWHselect" @click="whResetCatalogueFunction(3, 4, 'RS', 'D')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(3, 4).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(3, 4).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(3, 4).last.created_at ? find_WH_reset_row(3, 4).last.created_at : 'empty'
                        }}</span>
                    </td>


                    <td class="shiftWHselect" @click="whResetCatalogueFunction(4, 4, 'AM', 'D')">
                      <div style="margin-bottom: 3px;">
                        <span
                          style="padding:0 10px; border-right:1px solid #ffffff52; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #e74545;">mdi-arrow-down-bold</v-icon>: {{
                              find_WH_reset_row(4, 4).min }}(H)</span>
                        <span style="padding:0 10px; display: inline-block;width: 90px;"><v-icon
                            style="margin-top: -5px;font-size: 24px;color: #00b000;">mdi-arrow-up-bold</v-icon>: {{
                              find_WH_reset_row(4, 4).max }}(H)</span>
                      </div>
                      <span style="padding:0 10px; padding-top: 3px;"><v-icon
                          style="margin-top: -5px;font-size: 24px;color: rgb(57 61 255);">mdi-timer-refresh</v-icon>: {{
                            find_WH_reset_row(4, 4).last.created_at ? find_WH_reset_row(4, 4).last.created_at : 'empty'
                        }}</span>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </v-card-text>
          <template v-slot:actions>

            <v-spacer></v-spacer>

            <v-btn style=" background-color: #808080c2; color: white !important;" @click="resetWHShow = false"> CLOSE
            </v-btn>

          </template>
        </v-card>
      </v-dialog>
    </div>
  </template>
  <template>
    <div class="text-center pa-4">
      <v-dialog v-model="showResetWHConfirm" max-width="500" persistent>
        <v-card class="confirmDialogInterface" prepend-icon="mdi-clipboard-clock-outline"
          title="RESET THE WORKING HOURS">
          <v-card-text>
            Are you sure you want to reset the shift <span
              style=" font-size: 20px; font-weight: 700; color: #135e99; ">{{ shift_name }}</span> for <span
              style=" font-size: 20px; font-weight: 700; color: #135e99; ">{{ profileGroup_name }}</span>'s group
            wourking
            hours?
          </v-card-text>
          <template v-slot:actions>
            <v-btn style="display: none; background-color: rgb(61 137 51 / 76%); color: white !important;"
              @click="showResetWHConfirm = false"> Drivers list
            </v-btn>
            <v-btn style="display: none; background-color:rgb(134 59 181); color: white !important;"
              @click="showResetWHConfirm">
              WH History
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn style=" background-color: #808080c2; color: white !important;" @click="showResetWHConfirm = false">
              No
            </v-btn>
            <v-btn style=" background-color: rgb(74 63 244); color: white !important;" @click="resetOneTeamManually">
              RESET
            </v-btn>
          </template>
        </v-card>
      </v-dialog>
    </div>
  </template>
  <template>
    <div class="text-center">
      <v-snackbar v-model="snackbar" :timeout="timeout !== null ? timeout : undefined">
        <span v-html="text">

        </span>

        <template v-slot:actions>
          <v-btn color="blue" variant="text" @click="snackbar = false">
            Close
          </v-btn>
        </template>

      </v-snackbar>
    </div>
  </template>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import * as XLSX from 'xlsx';
export default {
  data: () => ({
    activeTab: 0,  // Default tab for switching between Personal Info, Work Info, and Status
      expanded: [],
    shift_name: '',
    shift_name_id: null,
    profileGroup_name: '',
    profileGroup_name_id: null,
    showResetWHConfirm: false,
    whResetCatalogue: [],
    resetWHShow: false,
    dialog: false,
    dialogDelete: false,
    dialogReset: false,
    editedItem: null,
    search: '',
    dialogType: '',
    snackbar: false,
    text: '',
    notAddedUsers: [],
    timeout: 2500,
    selectedValue: "",
    roles: [],
    shifts: [],
    profileGroups: [],
    file: {
      name: '',
    },
    reader: null,
    fileRead: false,
    user: {
      id: -1,
      matricule: "",
      firstname: "",
      lastname: "",
      email: "",
      workingHours: 0,
      sby_workingHours: 0,
      role_id: "",
      roleName: "",
      shift_id: "",
      shiftName: "",
      profile_group_id: "",
      profile_groupName: "",
      isactive: 1
    },
    headers: [
    {
        title: 'Matricule',
        sortable: true,
        key: 'matricule',
      },
      {
        title: 'Full Name',
        align: 'start',
        sortable: true,
        key: 'fullname',
      },

      { title: 'Email', key: 'email', sortable: true },
      {
        title: 'Role',
        sortable: true,
        key: 'role',
      },
      {
        title: 'Profile Group',
        sortable: true,
        key: 'profile_group',
      },
      {
        title: 'Shift',
        sortable: true,
        key: 'shift',
      },
      { title: 'Working Hours', key: 'workingHours', sortable: true },
      { title: 'Working Hours(Global)', key: 'wh_global', sortable: true },
      { title: 'SBY Working Hours(min)', key: 'sbyworkingHours', sortable: true },
      { title: 'Actions', key: 'actions', sortable: false },
    ],
    users: [],
    searchQuery: '',
    filteredUsers: [],
    submitClicked: false,
  }),
  computed: {
    ...mapGetters(["getUsers", "getToken", "getRoles", "getShifts", "getProfileGroups", "get_wh_all"]),
  },
  watch: {
    dialog(val) {
      val || this.close()
    },
    dialogDelete(val) {
      val || this.closeDelete()
    },
    dialogReset(val) {
      val || this.closeReset()
    },
    selectedValue(newValue) {
      console.log(newValue);
    }
  },
  mounted() {
    this.refreshUsersTable();
  },
  methods: {
    ...mapActions([
      "setUsersAction",
      "setLoadingValueAction",
      "resetPasswordAction",
      "deleteUserAction",
      "registerUserAction",
      "editUserAction",
      "setRolesAction",
      "setShiftsAction",
      "setProfileGroupsAction",
      "wh_index_action",
      "wh_reset_action",
    ]),
    resetOneTeamManually(equipment_type, shift) {
      this.wh_reset_action({
        "shift_id": this.shift_name_id,
        "profile_group_id": this.profileGroup_name_id
      })
        .then((e) => {
          console.log("diodoidsiojdsf ", this.get_wh_all);
          this.showResetWHConfirm = false;
        })
    },
    find_WH_reset_row(equipment_type, shift) {
      let rst = this.get_wh_all.filter((e) => {
        return e.equipment_type == equipment_type && e.shift == shift;
      });
      if (rst.length > 0) {
        return rst[0];
      }
      else {
        return {
          "min": 'empty',
          "max": 'empty',
          "last": 'empty',
          "equipment_type": 'empty',
          "shift": 'empty'
        };
      }
    },
    resetWHShowfunction() {
      this.wh_index_action()
        .then((e) => {
          console.log("diodoidsiojdsf ", this.get_wh_all);
          this.resetWHShow = true;
        })
    },
    whResetCatalogueFunction(profileGroup_, shift_, profileGroup_name, shift_name) {
      this.profileGroup_name_id = profileGroup_;
      this.shift_name_id = shift_;
      this.profileGroup_name = profileGroup_name;
      this.shift_name = shift_name;
      this.showResetWHConfirm = true;

    },
    requiredRule(field, fieldName) {
      return field || field === 0 ? true : `${fieldName.charAt(0).toUpperCase() + fieldName.slice(1)} field is required`;
    },
    refreshUsersTable(val) {
      this.setLoadingValueAction(true)
      this.setUsersAction()
        .then((response) => {
          this.users = this.getUsers;
          this.filteredUsers = this.users.map(item => ({
            fullname: item.firstname + " " + item.lastname,
            matricule: item.matricule,
            email: item.email,
            workingHours: item.workingHours || item.workingHours === 0 ? item.workingHours : 'undefined',
            wh_global: item.wh_global || item.wh_global === 0 ? item.wh_global : 'undefined',
            sby_workingHours: item.sby_workingHours || item.sby_workingHours === 0 ? item.sby_workingHours : 'undefined',
            roleName: item.role.name,
            profile_groupName: item.profile_group ? item.profile_group.type : 'none',
            shiftName: item.shift ? item.shift.category : 'none',
            firstname: item.firstname,
            lastname: item.lastname,
            isactive: item.isactive,
            id: item.id,
            role_id: item.role_id,
            shift_id: item.shift_id,
            profile_group_id: item.profile_group_id

          }))

          if (val) {
            this.text = val.text
            if (this.notAddedUsers.length > 0) {
              this.timeout = null
              this.snackbar = true
              this.notAddedUsers = []
            }
            else {
              this.timeout = 1500
              this.snackbar = true
            }

          }
        })
        .catch(error => {
          console.error("Set users error:", error);
          this.setLoadingValueAction(false)
        });
      this.setRolesAction().then(() => {
        this.roles = this.getRoles.map((role) => ({
          name: role.name,
          id: role.id
        }));
      });

      this.setShiftsAction().then(() => {
        this.setLoadingValueAction(false)
        this.shifts = this.getShifts.map((shift) => ({
          category: shift.category,
          id: shift.id
        })).sort((a, b) => a.category.localeCompare(b.category));
      });

      this.setProfileGroupsAction().then(() => {
        this.profileGroups = this.getProfileGroups.map((profileGroup) => ({
          type: profileGroup.type,
          id: profileGroup.id
        }))
      })

    },
    editItem(item) {
      this.editedItem = { ...item };
      this.editedIndex = item.id;
      this.dialogType = 'edit';
      this.dialog = true;
      // Populate other user details
      this.user.id = this.editedIndex;
      this.user.matricule = this.editedItem.matricule;
      this.user.firstname = this.editedItem.firstname;
      this.user.lastname = this.editedItem.lastname;
      this.user.email = this.editedItem.email;
      this.user.role_id = this.editedItem.role_id;
      this.user.shift_id = this.editedItem.shift_id;
      this.user.profile_group_id = this.editedItem.profile_group_id;
      this.user.isactive = String(this.editedItem.isactive);
    },
    resetItem(item) {
      this.editedItem = { ...item };
      this.editedIndex = item.id;
      this.dialogReset = true;
    },
    deleteItem(item) {
      this.editedItem = { ...item };
      this.editedIndex = item.id;
      this.dialogDelete = true;
    },
    deleteItemConfirm() {
      this.setLoadingValueAction(true)
      this.deleteUserAction(this.editedIndex).then((response) => {
        this.setLoadingValueAction(false)
        this.refreshUsersTable({ text: "user deleted successfully" })
      }).catch(error => {
        console.error("Delete user error:", error);
        this.setLoadingValueAction(false)
        this.text = "error deleting user"
        this.snackbar = true
      });
      this.closeDelete()
    },
    resetPasswordConfirm() {
      this.setLoadingValueAction(true)
      this.resetPasswordAction(this.editedIndex).then((response) => {
        console.log(response)
        this.setLoadingValueAction(false)
        this.refreshUsersTable({ text: "user password reset successfully" })
      }).catch(error => {
        console.error("Reset password error:", error);
        this.setLoadingValueAction(false)
        this.text = "error reseting user password"
        this.snackbar = true
      });
      this.closeReset()
    },
    close() {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
      this.dialogType = ''
      this.user.matricule = ''
      this.user.firstname = ''
      this.user.lastname = ''
      this.user.email = ''
      this.user.role_id = ''
      this.user.shift_id = ''
      this.user.profile_group_id = ''
      this.user.isactive = ''
    },
    closeDelete() {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },
    closeReset() {
      this.dialogReset = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },
    save() {
      this.submitClicked = true
      if (!this.user) {
        console.log("User Not Defined")
      }
      const userToSend = { ...this.user };
      delete userToSend.id;
      delete userToSend.profile_groupName;
      delete userToSend.shiftName;
      delete userToSend.roleName;

      console.log(userToSend);
      if (!userToSend) {
        console.log("Invalid user", userToSend)
        return;
      }
      else {
        if (this.dialogType == "edit") {
          this.setLoadingValueAction(true)
          this.editUserAction(this.user)
            .then((response) => {
              this.setLoadingValueAction(false)
              if (response)
                this.refreshUsersTable({ text: "user edited successfully" })
              else {
                this.text = "error editing user"
                this.snackbar = true
              }
            }).catch(error => {
              console.error("Edit user error:", error);
              this.setLoadingValueAction(false)
              this.text = "error editing user"
              this.snackbar = true
            });
          this.close()
        }
        else {
          delete userToSend.isactive;
          this.setLoadingValueAction(true)
          this.registerUserAction(userToSend).then((response) => {
            this.setLoadingValueAction(false)
            if (response)
              this.refreshUsersTable({ text: "user added successfully" })
            else {
              this.text = "error adding user"
              this.snackbar = true
            }
          }).catch(error => {
            console.error("Register user error:", error);
            this.setLoadingValueAction(false)
            this.text = "error adding user"
            this.snackbar = true
          });
          // console.log(userToSend)
          this.close()
        }

      }
    },
    openFileDialog() {
      this.$refs.fileInput.click();
    },
    resetWH() {
      if (this.users.length > 0) {
        let usersSorted = this.users.sort((a, b) => {
          return a.workingHours - b.workingHours;
        });
        console.log(usersSorted[0]);
        if (usersSorted[0].workingHours < 0) {
          usersSorted[0].workingHours = 0;
        }
      }
    },
    importUsers(reader, file) {
      reader.onload = (e) => {
        const data = new Uint8Array(e.target.result); // Convert file data to Uint8Array
        const workbook = XLSX.read(data, { type: 'array' }); // Read the Excel file
        const sheetName = workbook.SheetNames[0]; // Get the name of the first sheet
        const worksheet = workbook.Sheets[sheetName]; // Get the worksheet
        const jsonData = XLSX.utils.sheet_to_json(worksheet, { header: 1 }); // Convert worksheet to JSON

        this.setLoadingValueAction(true);
        const promises = [];
        const usersList = [];
        for (const rowData of jsonData.slice(1)) { // Start from index 1 to skip header row
          const [matricule, firstname, lastname, email, roleName, profileGroupName, shiftCategory, workingHours, sby_workingHours] = rowData;
          // console.log(roleName + " " + profileGroupName + " " + shiftCategory);
          const role = this.roles.find(role => role.name === roleName);
          const profileGroup = this.profileGroups.find(profileGroup => profileGroup.type === profileGroupName);
          const shift = this.shifts.find(shift => shift.category === shiftCategory);
          if (role && profileGroup && shift) {
            const user = {
              matricule,
              firstname,
              lastname,
              email,
              role_id: role.id,
              profile_group_id: profileGroup.id,
              shift_id: shift.id,
              workingHours,
              sby_workingHours
            };
            usersList.push(user);
            ///const promise = this.registerUserAction(user)
            ///  .then(response => {
            ///    if (!response) {
            ///      this.notAddedUsers.push(user.matricule);
            ///    }
            ///  })
            ///  .catch(error => {
            ///    console.log("Error adding user:", error);
            ///    this.notAddedUsers.push(user.matricule);
            ///  });
            ///
            //promises.push(promise);
          } else {
            console.warn("Role, profile group, or shift not found for row:", rowData);
          }
        }
        console.log(usersList);
        //Promise.all(promises)
        //  .then(() => {
        //    this.setLoadingValueAction(false);
        //    this.refreshUsersTable({ text: "Importing users done" + '<br>' + "Error adding users: " + (this.notAddedUsers.length > 0 ? this.notAddedUsers.join(", ") : "none") });
        //    this.reader = null;
        //    this.file = { name: "" };
        //  });
      };
      reader.readAsArrayBuffer(file);
    },
    handleFileUpload(event) {
      this.file = event.target.files[0]; // Get the selected file
      this.reader = new FileReader();
      this.importUsers(this.reader, this.file);
      event.target.value = null; // Reset the file input
    }
  },
}
</script>
<style scoped>
.expanded-row-card {
  padding: 16px;
  margin-top: 8px;
  border-radius: 10px;
  background-color: #ffffff;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

.action-buttons-row {
  margin-top: 16px;
}

.v-chip {
  margin-right: 5px;
}

.v-avatar {
  margin-bottom: 8px;
}
.container {
  display: grid;
  grid-template-rows: 10vh 10vh 70vh;
}

.addUserButtonZone {
  background-color: white;
  height: 30px;
  margin-bottom: 2rem;
}

.addUserButton {
  border: 1px solid gray;
  background-color: darkblue;
}

.addUserButtonStyle {
  width: fit-content;
  display: flex;
  justify-content: end;
  align-items: center;
}

.parentContainer {
  width: 100%;
  scroll-behavior: none;
}

.searchContainer {
  margin: auto;
  width: 60vw;
}

.dialogText {
  font-size: medium;
  background-color: white;
  text-align: center;
}

.dialogCancel {
  background-color: red;
  color: white;
}

.dialogCancel:hover {
  background-color: darkred;
}

.dialogOk {
  background-color: blue;
  color: white;
}

.dialogOk:hover {
  background-color: darkblue;
}

.dialogAddUserTitleContainer {
  display: flex;
  width: 100%;
  justify-content: start;
  margin-left: 15px;
  padding-top: 15px;
}

.actions {
  display: flex;
  justify-content: start;
  align-items: center;
}

.icon {
  margin: 6px;
}

.inactive-user {
  color: lightsalmon;
}

.dialog {
  z-index: 20;
}

.dialogTop {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.importfile {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 6px;
}

.importfile p {
  font-size: small;
}

.addActions {
  width: 100%;
  display: flex;
  justify-content: end;
  align-items: center;
  padding-right: 5%;
  gap: 2%;
}

.importUsersButton {
  border: 1px solid gray;
  background-color: black;
}
</style>
