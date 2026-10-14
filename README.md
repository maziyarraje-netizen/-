<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>کافه آرامش | رزرو میز</title>

  <!-- Quasar + Material Icons -->
  <link href="https://cdn.jsdelivr.net/npm/quasar@2.18.5/dist/quasar.rtl.prod.css" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css2?family=Vazirmatn:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <link href="style.css" rel="stylesheet">
</head>

<body>
  <div id="app">
    <q-layout view="lHh Lpr lFf">

      <!-- Header -->
      <q-header class="app-header">
        <q-toolbar class="container">
          <q-toolbar-title class="brand">
            ☕ کافه آرامش
          </q-toolbar-title>

          <div class="desktop-menu">
            <q-btn flat label="خانه" @click="scrollTo('home')"></q-btn>
            <q-btn flat label="رزرو میز" @click="scrollTo('reservation')"></q-btn>
            <q-btn flat label="منوی کافه" @click="scrollTo('menu')"></q-btn>
            <q-btn flat label="رزرو های من" @click="scrollTo('my-reservations')"></q-btn>
          </div>

          <q-btn
            flat
            round
            class="mobile-menu-btn"
            @click="mobileMenu = !mobileMenu"
          ></q-btn>
        </q-toolbar>

        <div v-if="mobileMenu" class="mobile-menu">
          <q-btn flat label="خانه" @click="scrollTo('home'); mobileMenu=false"></q-btn>
          <q-btn flat label="رزرو میز" @click="scrollTo('reservation'); mobileMenu=false"></q-btn>
          <q-btn flat label="منوی کافه" @click="scrollTo('menu'); mobileMenu=false"></q-btn>
          <q-btn flat label="رزرو های من" @click="scrollTo('my-reservations'); mobileMenu=false"></q-btn>
        </div>
      </q-header>

      <q-page-container>
        <q-page>

          <!-- Hero -->
          <section id="home" class="hero-section">
            <div class="container hero-content">
              <div class="hero-text">
                <span class="eyebrow">یک میز برای یک لحظه خوب</span>
                <h1>به کافه آرامش خوش آمدید</h1>
                <p>
                  میز موردنظرتان را انتخاب کنید، زمان حضورتان را مشخص کنید
                  و با چند کلیک رزروتان را انجام دهید.
                </p>
                <q-btn
                  unelevated
                  class="main-btn"
                  label="رزرو میز"
                  icon=""
                  @click="scrollTo('reservation')"
                ></q-btn>
              </div>

              <div class="hero-card">
                <div class="hero-card-icon">☕</div>
                <strong>فضایی گرم و آرام</strong>
                <span>برای قرارهای دوستانه و خانوادگی</span>
              </div>
            </div>
          </section>

          <!-- Reservation -->
          <section id="reservation" class="section">
            <div class="container">
              <div class="section-title">
                <span>رزرو میز</span>
                <h2>میز مناسب خودت رو پیدا کن</h2>
                <p>اول تعداد نفرات رو انتخاب کن تا میزهای مناسب نمایش داده بشن.</p>
              </div>

              <!-- Guest filter -->
              <div class="filter-box">
                <div>
                  <div class="filter-title">تعداد نفرات</div>
                  <div class="filter-subtitle">میز مناسب مهمانی شما</div>
                </div>

                <q-select
                  v-model="selectedCapacity"
                  :options="capacityOptions"
                  emit-value
                  map-options
                  outlined
                  dense
                  class="capacity-select"
                  label="انتخاب تعداد نفرات"
                ></q-select>
              </div>

              <!-- Tables -->
              <div class="tables-grid">
                <q-card
                  v-for="table in filteredTables"
                  :key="table.id"
                  class="table-card"
                  :class="{ reserved: table.isReserved }"
                >
                  <q-card-section>
                    <div class="table-top">
                      <div class="table-number">میز {{ table.id }}</div>

                      <q-badge
                        v-if="table.isReserved"
                        color="negative"
                        label="رزرو شده"
                        class="status-badge"
                      ></q-badge>

                      <q-badge
                        v-else
                        color="positive"
                        label="آماده رزرو"
                        class="status-badge"
                      ></q-badge>
                    </div>

                    <div class="table-icon">🪑</div>

                    <h3>{{ table.title }}</h3>

                    <div class="capacity">
                      <span class="people-icon">👥</span> ظرفیت: {{ table.capacity }} نفر
                    </div>
                  </q-card-section>

                  <q-card-actions>
                    <q-btn
                      unelevated
                      class="reserve-btn"
                      label="انتخاب این میز"
                      :disable="table.isReserved"
                      @click="openReservation(table)"
                    ></q-btn>
                  </q-card-actions>
                </q-card>
              </div>

              <div v-if="filteredTables.length === 0" class="empty-state">
                <div>☕</div>
                <h3>میزی پیدا نشد</h3>
                <p>برای این تعداد نفرات میز خالی نداریم.</p>
              </div>
            </div>
          </section>

          <!-- Menu -->
          <section id="menu" class="section menu-section">
            <div class="container">
              <div class="section-title">
                <span>منوی کافه</span>
                <h2>یه چیزی هم برای خوردن انتخاب کن</h2>
                <p>چند مورد از آیتم‌های محبوب کافه آرامش</p>
              </div>

              <div class="menu-grid">
                <q-card v-for="item in menuItems" :key="item.name" class="menu-card">
                  <div class="menu-image">{{ item.icon }}</div>
                  <q-card-section>
                    <h3>{{ item.name }}</h3>
                    <p>{{ item.description }}</p>
                    <div class="menu-price">{{ item.price }} تومان</div>
                  </q-card-section>
                </q-card>
              </div>
            </div>
          </section>


          <!-- My Reservations -->
          <section id="my-reservations" class="section">
            <div class="container">
              <div class="section-title">
                <span>رزرو های من</span>
                <h2>رزروهای ثبت‌شده</h2>
                <p>رزروهای خودت رو اینجا می‌بینی و در صورت نیاز می‌تونی لغوشون کنی.</p>
              </div>

              <div v-if="myReservations.length > 0">
                <q-card
                  v-for="reservation in myReservations"
                  :key="reservation.tableId"
                  class="my-reservation-card"
                >
                  <q-card-section>
                    <div class="table-top">
                      <div class="table-number">{{ reservation.tableTitle }}</div>
                      <q-badge color="positive" label="فعال"></q-badge>
                    </div>

                    <div class="q-mt-sm">
                      <div>🪑 میز {{ reservation.tableId }}</div>
                      <div>🕐 زمان حضور: {{ reservation.time }}</div>
                      <div>👤 نام: {{ reservation.firstName }} {{ reservation.lastName }}</div>
                      <div>📞 شماره تلفن: {{ reservation.phone }}</div>
                    </div>
                  </q-card-section>

                  <q-card-actions align="left">
                    <q-btn
                      unelevated
                      color="negative"
                      label="لغو رزرو"
                      @click="cancelReservation(reservation)"
                    ></q-btn>
                  </q-card-actions>
                </q-card>
              </div>

              <div v-else class="empty-state">
                <div>☕</div>
                <h3>هنوز رزروی نداری</h3>
                <p>بعد از ثبت رزرو، اطلاعات میز اینجا نمایش داده می‌شود.</p>
              </div>
            </div>
          </section>

          <!-- Footer -->
          <footer class="footer">
            <div class="container">
              <strong>☕ کافه آرامش</strong>
              <span>سامانه رزرو میز کافه</span>
            </div>
          </footer>

          <!-- Reservation Dialog -->
          <q-dialog v-model="dialogOpen" persistent>
            <q-card class="reservation-dialog">
              <q-card-section class="dialog-header">
                <div>
                  <div class="dialog-eyebrow">رزرو میز</div>
                  <h2>{{ selectedTable ? selectedTable.title : '' }}</h2>
                </div>

                <q-btn
                  flat
                  round
                  v-close-popup
                  @click="resetForm"
                ></q-btn>
              </q-card-section>

              <q-card-section v-if="selectedTable">
                <div class="selected-table-info">
                  <span>میز {{ selectedTable.id }}</span>
                  <span>ظرفیت {{ selectedTable.capacity }} نفر</span>
                </div>

                <q-form @submit.prevent="submitReservation" class="reservation-form">
                  <q-input
                    v-model="customerName"
                    outlined
                    label="نام"
                    placeholder="مثلاً مبینا"
                    :rules="[val => !!val || 'نام را وارد کنید']"
                  ></q-input>

                  <q-input
                    v-model="customerLastName"
                    outlined
                    label="نام خانوادگی"
                    placeholder="مثلاً رضایی"
                    :rules="[val => !!val || 'نام خانوادگی را وارد کنید']"
                  ></q-input>

                  <q-input
                    v-model="customerPhone"
                    outlined
                    type="tel"
                    label="شماره تلفن"
                    placeholder="مثلاً 09123456789"
                    :rules="[val => isPhoneValid || 'شماره تلفن باید دقیقاً ۱۱ رقم باشد']"
                  ></q-input>

                  <div v-if="customerPhone && !isPhoneValid" class="time-error">
                    شماره تلفن باید دقیقاً ۱۱ رقم باشد.
                  </div>

                  <div class="time-title">زمان حضور</div>

                  <div class="time-slots">
                    <q-btn
                      v-for="time in timeSlots"
                      :key="time"
                      unelevated
                      :label="time"
                      class="time-btn"
                      :class="{ selected: selectedTime === time }"
                      @click="selectedTime = time"
                    ></q-btn>
                  </div>

                  <div v-if="!selectedTime" class="time-error">
                    لطفاً یک زمان را انتخاب کنید.
                  </div>

                  <q-btn
                    type="submit"
                    unelevated
                    class="submit-btn"
                    label="ثبت رزرو"
                    icon=""
                    :class="{ 'phone-invalid': !isPhoneValid }"
                    :disable="!selectedTime || !customerName.trim() || !customerLastName.trim() || !isPhoneValid"
                  ></q-btn>
                </q-form>
              </q-card-section>
            </q-card>
          </q-dialog>

        </q-page>
      </q-page-container>
    </q-layout>
  </div>

  <!-- Vue + Quasar -->
  <script src="https://cdn.jsdelivr.net/npm/vue@3/dist/vue.global.prod.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/quasar@2.18.5/dist/quasar.umd.prod.js"></script>
  <script src="script.js"></script>
</body>
</html>
