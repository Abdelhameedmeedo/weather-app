<template>
 <div id="app">
    <div class="weather-app">
      <div class="container">
        <div class="row">
          <div class="col-lg-12 sm-4">
              <form action="" @submit.prevent="get_data">
                  <input type="text" v-model="search" class="input" ref="focus" placeholder="search for city...">
              </form>
          </div>
          <div class="col-lg-8 sm-4">
            <div class="weather-content" :class="day? 'today':'tonight'">
              <ul>
                <li class="city">{{`${weather.city} - ${weather.country}`}}</li>
                <li class="date">{{ `${month_number}/${day_number} ${day_name}` }}</li>
              </ul>
              <h3 class="temp">{{weather.temprature}}&#176;c</h3>
              <p class="desc">{{weather.description}}</p>
            </div>
          </div>        
        </div><!--end row-->
      </div><!--end container-->
    </div>
 </div>
</template>

<script>
//openweather => `https://api.openweathermap.org/data/2.5/weather?q=${this.saerch}&appid=${api_key}`
//timezone=> https://api.timezonedb.com/v2.1/list-time-zone?key=R7Z6HQ15OSMV&format=xml&country=EG
export default {
  name: 'App',

  data(){
    return{
      search:'',
      isDay:true,
      day_name:'day here',
      day_number:1,
      month_number:1,
     weather:{
      city:'cairo',
      country:'EG',
      humidity:'0',
      temprature:'0',
      description:'sunny',
      erorrCity:''
     }
     
    }
  },
  mounted(){
    this.$refs.focus.focus()
  },
  methods:{
    get_data:async function(){
      const days = ['sunday','monday','tuesday','wednesday','thursday','friday','saturday']
    //  const months = ["January","February","March","April","May","June","July","August","September","October","November","December"];
      const openweather_api_key = 'd456bcbafc353b91436bad6573aa617b'
      const api_url = `https://api.openweathermap.org/data/2.5/weather?q=${this.search}&appid=${openweather_api_key}&units=metric`
      const fetchData = await fetch(api_url)
      const get_data = await fetchData.json()
      console.log('data',get_data)
      this.weather.city = get_data.name
      this.weather.country = get_data.sys.country
      this.weather.humidity = Math.round(get_data.main.humidity)
      this.weather.temprature = Math.round(get_data.main.temp)
      this.weather.description = get_data.weather[0].description
      //date
      let get_date = new Date()
      let get_day = get_date.getDay() //day number of the week
      let get_day_number_inmonth = get_date.getDate() //day number of the month
      let get_month = get_date.getMonth()+1
      this.day_number = get_day_number_inmonth
      this.month_number = get_month
      for(let i=0;i<days.length;i++){
        let day_name = days[get_day]
        this.day_name = day_name
      }
      //wrong city
     if(this.weather.city == 'undefined'){
        this.weather.city = 'hello'
     }
      //day or night
      const dayORnight = get_data.weather[0].icon
      if(dayORnight.includes('n')){
        this.isDay = false
      }else{
        this.isDay = true
      }
      //get countries name
      let countries=  [ 
        {"name": "Afghanistan", "code": "AF"}, 
        {"name": "land Islands", "code": "AX"}, 
        {"name": "Albania", "code": "AL"}, 
        {"name": "Algeria", "code": "DZ"}, 
        {"name": "American Samoa", "code": "AS"}, 
        {"name": "AndorrA", "code": "AD"}, 
        {"name": "Angola", "code": "AO"}, 
        {"name": "Anguilla", "code": "AI"}, 
        {"name": "Antarctica", "code": "AQ"}, 
        {"name": "Antigua and Barbuda", "code": "AG"}, 
        {"name": "Argentina", "code": "AR"}, 
        {"name": "Armenia", "code": "AM"}, 
        {"name": "Aruba", "code": "AW"}, 
        {"name": "Australia", "code": "AU"}, 
        {"name": "Austria", "code": "AT"}, 
        {"name": "Azerbaijan", "code": "AZ"}, 
        {"name": "Bahamas", "code": "BS"}, 
        {"name": "Bahrain", "code": "BH"}, 
        {"name": "Bangladesh", "code": "BD"}, 
        {"name": "Barbados", "code": "BB"}, 
        {"name": "Belarus", "code": "BY"}, 
        {"name": "Belgium", "code": "BE"}, 
        {"name": "Belize", "code": "BZ"}, 
        {"name": "Benin", "code": "BJ"}, 
        {"name": "Bermuda", "code": "BM"}, 
        {"name": "Bhutan", "code": "BT"}, 
        {"name": "Bolivia", "code": "BO"}, 
        {"name": "Bosnia and Herzegovina", "code": "BA"}, 
        {"name": "Botswana", "code": "BW"}, 
        {"name": "Bouvet Island", "code": "BV"}, 
        {"name": "Brazil", "code": "BR"}, 
        {"name": "British Indian Ocean Territory", "code": "IO"}, 
        {"name": "Brunei Darussalam", "code": "BN"}, 
        {"name": "Bulgaria", "code": "BG"}, 
        {"name": "Burkina Faso", "code": "BF"}, 
        {"name": "Burundi", "code": "BI"}, 
        {"name": "Cambodia", "code": "KH"}, 
        {"name": "Cameroon", "code": "CM"}, 
        {"name": "Canada", "code": "CA"}, 
        {"name": "Cape Verde", "code": "CV"}, 
        {"name": "Cayman Islands", "code": "KY"}, 
        {"name": "Central African Republic", "code": "CF"}, 
        {"name": "Chad", "code": "TD"}, 
        {"name": "Chile", "code": "CL"}, 
        {"name": "China", "code": "CN"}, 
        {"name": "Christmas Island", "code": "CX"}, 
        {"name": "Cocos (Keeling) Islands", "code": "CC"}, 
        {"name": "Colombia", "code": "CO"}, 
        {"name": "Comoros", "code": "KM"}, 
        {"name": "Congo", "code": "CG"}, 
        {"name": "Congo, The Democratic Republic of the", "code": "CD"}, 
        {"name": "Cook Islands", "code": "CK"}, 
        {"name": "Costa Rica", "code": "CR"}, 
        {"name": "Cote D’Ivoire", "code": "CI"}, 
        {"name": "Croatia", "code": "HR"}, 
        {"name": "Cuba", "code": "CU"}, 
        {"name": "Cyprus", "code": "CY"}, 
        {"name": "Czech Republic", "code": "CZ"}, 
        {"name": "Denmark", "code": "DK"}, 
        {"name": "Djibouti", "code": "DJ"}, 
        {"name": "Dominica", "code": "DM"}, 
        {"name": "Dominican Republic", "code": "DO"}, 
        {"name": "Ecuador", "code": "EC"}, 
        {"name": "Egypt", "code": "EG"}, 
        {"name": "El Salvador", "code": "SV"}, 
        {"name": "Equatorial Guinea", "code": "GQ"}, 
        {"name": "Eritrea", "code": "ER"}, 
        {"name": "Estonia", "code": "EE"}, 
        {"name": "Ethiopia", "code": "ET"}, 
        {"name": "Falkland Islands (Malvinas)", "code": "FK"}, 
        {"name": "Faroe Islands", "code": "FO"}, 
        {"name": "Fiji", "code": "FJ"}, 
        {"name": "Finland", "code": "FI"}, 
        {"name": "France", "code": "FR"}, 
        {"name": "French Guiana", "code": "GF"}, 
        {"name": "French Polynesia", "code": "PF"}, 
        {"name": "French Southern Territories", "code": "TF"}, 
        {"name": "Gabon", "code": "GA"}, 
        {"name": "Gambia", "code": "GM"}, 
        {"name": "Georgia", "code": "GE"}, 
        {"name": "Germany", "code": "DE"}, 
        {"name": "Ghana", "code": "GH"}, 
        {"name": "Gibraltar", "code": "GI"}, 
        {"name": "Greece", "code": "GR"}, 
        {"name": "Greenland", "code": "GL"}, 
        {"name": "Grenada", "code": "GD"}, 
        {"name": "Guadeloupe", "code": "GP"}, 
        {"name": "Guam", "code": "GU"}, 
        {"name": "Guatemala", "code": "GT"}, 
        {"name": "Guernsey", "code": "GG"}, 
        {"name": "Guinea", "code": "GN"}, 
        {"name": "Guinea-Bissau", "code": "GW"}, 
        {"name": "Guyana", "code": "GY"}, 
        {"name": "Haiti", "code": "HT"}, 
        {"name": "Heard Island and Mcdonald Islands", "code": "HM"}, 
        {"name": "Holy See (Vatican City State)", "code": "VA"}, 
        {"name": "Honduras", "code": "HN"}, 
        {"name": "Hong Kong", "code": "HK"}, 
        {"name": "Hungary", "code": "HU"}, 
        {"name": "Iceland", "code": "IS"}, 
        {"name": "India", "code": "IN"}, 
        {"name": "Indonesia", "code": "ID"}, 
        {"name": "Iran, Islamic Republic Of", "code": "IR"}, 
        {"name": "Iraq", "code": "IQ"}, 
        {"name": "Ireland", "code": "IE"}, 
        {"name": "Isle of Man", "code": "IM"}, 
        {"name": "Israel", "code": "IL"}, 
        {"name": "Italy", "code": "IT"}, 
        {"name": "Jamaica", "code": "JM"}, 
        {"name": "Japan", "code": "JP"}, 
        {"name": "Jersey", "code": "JE"}, 
        {"name": "Jordan", "code": "JO"}, 
        {"name": "Kazakhstan", "code": "KZ"}, 
        {"name": "Kenya", "code": "KE"}, 
        {"name": "Kiribati", "code": "KI"}, 
        {"name": "Korea, Democratic People", "code": "KP"}, 
        {"name": "Korea, Republic of", "code": "KR"}, 
        {"name": "Kuwait", "code": "KW"}, 
        {"name": "Kyrgyzstan", "code": "KG"}, 
        {"name": "Latvia", "code": "LV"}, 
        {"name": "Lebanon", "code": "LB"}, 
        {"name": "Lesotho", "code": "LS"}, 
        {"name": "Liberia", "code": "LR"}, 
        {"name": "Libyan Arab Jamahiriya", "code": "LY"}, 
        {"name": "Liechtenstein", "code": "LI"}, 
        {"name": "Lithuania", "code": "LT"}, 
        {"name": "Luxembourg", "code": "LU"}, 
        {"name": "Macao", "code": "MO"}, 
        {"name": "Macedonia, The Former Yugoslav Republic of", "code": "MK"}, 
        {"name": "Madagascar", "code": "MG"}, 
        {"name": "Malawi", "code": "MW"}, 
        {"name": "Malaysia", "code": "MY"}, 
        {"name": "Maldives", "code": "MV"}, 
        {"name": "Mali", "code": "ML"}, 
        {"name": "Malta", "code": "MT"}, 
        {"name": "Marshall Islands", "code": "MH"}, 
        {"name": "Martinique", "code": "MQ"}, 
        {"name": "Mauritania", "code": "MR"}, 
        {"name": "Mauritius", "code": "MU"}, 
        {"name": "Mayotte", "code": "YT"}, 
        {"name": "Mexico", "code": "MX"}, 
        {"name": "Micronesia, Federated States of", "code": "FM"}, 
        {"name": "Moldova, Republic of", "code": "MD"}, 
        {"name": "Monaco", "code": "MC"}, 
        {"name": "Mongolia", "code": "MN"}, 
        {"name": "Montenegro", "code": "ME"},
        {"name": "Montserrat", "code": "MS"},
        {"name": "Morocco", "code": "MA"}, 
        {"name": "Mozambique", "code": "MZ"}, 
        {"name": "Myanmar", "code": "MM"}, 
        {"name": "Namibia", "code": "NA"}, 
        {"name": "Nauru", "code": "NR"}, 
        {"name": "Nepal", "code": "NP"}, 
        {"name": "Netherlands", "code": "NL"}, 
        {"name": "Netherlands Antilles", "code": "AN"}, 
        {"name": "New Caledonia", "code": "NC"}, 
        {"name": "New Zealand", "code": "NZ"}, 
        {"name": "Nicaragua", "code": "NI"}, 
        {"name": "Niger", "code": "NE"}, 
        {"name": "Nigeria", "code": "NG"}, 
        {"name": "Niue", "code": "NU"}, 
        {"name": "Norfolk Island", "code": "NF"}, 
        {"name": "Northern Mariana Islands", "code": "MP"}, 
        {"name": "Norway", "code": "NO"}, 
        {"name": "Oman", "code": "OM"}, 
        {"name": "Pakistan", "code": "PK"}, 
        {"name": "Palau", "code": "PW"}, 
        {"name": "Palestinian Territory, Occupied", "code": "PS"}, 
        {"name": "Panama", "code": "PA"}, 
        {"name": "Papua New Guinea", "code": "PG"}, 
        {"name": "Paraguay", "code": "PY"}, 
        {"name": "Peru", "code": "PE"}, 
        {"name": "Philippines", "code": "PH"}, 
        {"name": "Pitcairn", "code": "PN"}, 
        {"name": "Poland", "code": "PL"}, 
        {"name": "Portugal", "code": "PT"}, 
        {"name": "Puerto Rico", "code": "PR"}, 
        {"name": "Qatar", "code": "QA"}, 
        {"name": "Reunion", "code": "RE"}, 
        {"name": "Romania", "code": "RO"}, 
        {"name": "Russian Federation", "code": "RU"}, 
        {"name": "RWANDA", "code": "RW"}, 
        {"name": "Saint Helena", "code": "SH"}, 
        {"name": "Saint Kitts and Nevis", "code": "KN"}, 
        {"name": "Saint Lucia", "code": "LC"}, 
        {"name": "Saint Pierre and Miquelon", "code": "PM"}, 
        {"name": "Saint Vincent and the Grenadines", "code": "VC"}, 
        {"name": "Samoa", "code": "WS"}, 
        {"name": "San Marino", "code": "SM"}, 
        {"name": "Sao Tome and Principe", "code": "ST"}, 
        {"name": "Saudi Arabia", "code": "SA"}, 
        {"name": "Senegal", "code": "SN"}, 
        {"name": "Serbia", "code": "RS"}, 
        {"name": "Seychelles", "code": "SC"}, 
        {"name": "Sierra Leone", "code": "SL"}, 
        {"name": "Singapore", "code": "SG"}, 
        {"name": "Slovakia", "code": "SK"}, 
        {"name": "Slovenia", "code": "SI"}, 
        {"name": "Solomon Islands", "code": "SB"}, 
        {"name": "Somalia", "code": "SO"}, 
        {"name": "South Africa", "code": "ZA"}, 
        {"name": "South Georgia and the South Sandwich Islands", "code": "GS"}, 
        {"name": "Spain", "code": "ES"}, 
        {"name": "Sri Lanka", "code": "LK"}, 
        {"name": "Sudan", "code": "SD"}, 
        {"name": "Suriname", "code": "SR"}, 
        {"name": "Svalbard and Jan Mayen", "code": "SJ"}, 
        {"name": "Swaziland", "code": "SZ"}, 
        {"name": "Sweden", "code": "SE"}, 
        {"name": "Switzerland", "code": "CH"}, 
        {"name": "Syrian Arab Republic", "code": "SY"}, 
        {"name": "Taiwan, Province of China", "code": "TW"}, 
        {"name": "Tajikistan", "code": "TJ"}, 
        {"name": "Tanzania, United Republic of", "code": "TZ"}, 
        {"name": "Thailand", "code": "TH"}, 
        {"name": "Timor-Leste", "code": "TL"}, 
        {"name": "Togo", "code": "TG"}, 
        {"name": "Tokelau", "code": "TK"}, 
        {"name": "Tonga", "code": "TO"}, 
        {"name": "Trinidad and Tobago", "code": "TT"}, 
        {"name": "Tunisia", "code": "TN"}, 
        {"name": "Turkey", "code": "TR"}, 
        {"name": "Turkmenistan", "code": "TM"}, 
        {"name": "Turks and Caicos Islands", "code": "TC"}, 
        {"name": "Tuvalu", "code": "TV"}, 
        {"name": "Uganda", "code": "UG"}, 
        {"name": "Ukraine", "code": "UA"}, 
        {"name": "United Arab Emirates", "code": "AE"}, 
        {"name": "United Kingdom", "code": "GB"}, 
        {"name": "United States", "code": "US"}, 
        {"name": "United States Minor Outlying Islands", "code": "UM"}, 
        {"name": "Uruguay", "code": "UY"}, 
        {"name": "Uzbekistan", "code": "UZ"}, 
        {"name": "Vanuatu", "code": "VU"}, 
        {"name": "Venezuela", "code": "VE"}, 
        {"name": "VietNam", "code": "VN"}, 
        {"name": "Virgin Islands, British", "code": "VG"}, 
        {"name": "Virgin Islands, U.S.", "code": "VI"}, 
        {"name": "Wallis and Futuna", "code": "WF"}, 
        {"name": "Western Sahara", "code": "EH"}, 
        {"name": "Yemen", "code": "YE"}, 
        {"name": "Zambia", "code": "ZM"}, 
        {"name": "Zimbabwe", "code": "ZW"} 
      ]
      countries.forEach((el)=>{
        let lower_char = el.name.charAt(0).toLocaleLowerCase()+el.name.slice(1)
        if(this.search == lower_char){
          console.log(el.name,lower_char)
          this.weather.city = 'error'
          this.weather.country = el.code
        }
        if(this.weather.country == el.code){
          this.weather.country = el.name
        }
      })
    }
  }

}
</script>

<style lang="scss">
// npm install sass-loader -D
@import '@/sass/weather.scss';
</style>
