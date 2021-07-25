<template>
  <div class="app">
    <!-- When you paste the function download() instead downloadJSON(), app get request at Pixlpark server-->
    <button class="download" @click="downloadJSON()" :class="{ none: isInvis }">
      Загрузить данные
    </button>
    <section class="content">
      <article class="item" v-for="order in orders" :key="order.Id">
        <h2 class="item__header">{{ order.Title }}</h2>
        <img
          class="item__img"
          :src="`` + order.PreviewImageUrl + ``"
          alt="Изображение товара"
        />
        <div class="item__statuses">
          <p class="item__status" v-if="order.Status == 'NotProcessed'">
            Статус заказа: Не начат
          </p>
          <p class="item__paid-status" v-if="order.PaymentStatus == 'NotPaid'">
            Статус оплаты: Не оплачен
          </p>
        </div>
         <p class="item__subheader">Данные об адресе доставки и получателе</p>
        <!-- I use more one directive v-for with v-if cuz template somehow can't see fields of the object inside the object and get Error in render: "TypeError: Cannot read property 'FullName' of null". -->
        <div
          class="item__delivery"
          v-for="item in order.DeliveryAddress"
          :key="item.id"
        >
          <p
            class="item__customer-name"
            v-if="item == order.DeliveryAddress.FullName"
          >
            Имя: {{ order.DeliveryAddress.FullName }}
          </p>
          <p
            class="item__customer-phone"
            v-if="item == order.DeliveryAddress.Phone"
          >
            Номер заказчика: {{ order.DeliveryAddress.Phone }}
          </p>
          <p class="item__address">
            <span
              class="item__country"
              v-if="item == order.DeliveryAddress.Country"
              >{{ order.DeliveryAddress.Country }}</span
            >
            <span
              class="item__city"
              v-if="item == order.DeliveryAddress.City"
              >{{ order.DeliveryAddress.City }}</span
            >
            <span
              class="item__street"
              v-if="item == order.DeliveryAddress.AddressLine1"
              >{{ order.DeliveryAddress.AddressLine1 }}</span
            >
            <span class="item__zip" v-if="item == order.DeliveryAddress.ZipCode && item != '' "
              >Адрес доставки: {{ order.DeliveryAddress.ZipCode }}</span
            >
          </p>
        </div>
         <p class="item__subheader">Данные о типе доставки</p>
        <div
          class="item__shipping"
          v-for="item in order.Shipping"
          :key="item"
        >
          <p class="item__shipping-type" v-if="item == order.Shipping.Title">
             {{ order.Shipping.Title }}
          </p>
          <p
            class="item__shipping-phone"
            v-if="item == order.Shipping.Phone && item != null"
          >
            Номер: {{ order.Shipping.Phone }}
          </p>
        </div>
      </article>
    </section>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function () {
    return {
      requestToken: "",
      accessToken: "",
      orders: [],
      isInvis: false,
    };
  },
  methods: {
    download: function () {
      this.request();
      if (this.requestToken != "") {
        this.access();
      }
      if (this.accessToken != "") {
        fetch(
          `http://api.pixlpark.com/orders?oauth_token=${this.accessToken}&take`
        ).then((response) => {
          // console.log(response);
          return (this.orders = response.json().Result);
        });
      }
    },
    request: function () {
      fetch("http://api.pixlpark.com/oauth/requesttoken", {
        // mode: "no-cors",
      }).then((response) => {
        // console.log(response);
        return (this.requesttoken = response.json().RequestToken);
      });
    },
    access: function () {
      fetch(
        `http://api.pixlpark.com/oauth/accesstoken?oauth_token=${this.requesttoken}&grant_type=api&username=38cd79b5f2b2486d86f562e3c43034f8&password=${this.requesttoken}8e49ff607b1f46e1a5e8f6ad5d312a80`,
        {
          // mode: "no-cors",
          credentials: "include",
        }
      ).then((res) => {
        // console.log(res);
        return (this.accessToken = res.json().AccessToken);
      });
    },
    downloadJSON: function () {
      const ordersJSON = `{
    "ApiVersion": "1.0",
    "Result": [
        {
            "Id": "2882993",
            "PhotolabId": 1164,
            "CustomId": "2882993",
            "SourceOrderId": null,
            "ManagerId": null,
            "AssignedToId": null,
            "Title": "Фотокниги в мягкой обложке (15x20 см)",
            "TrackingUrl": "",
            "TrackingNumber": null,
            "Status": "NotProcessed",
            "RenderStatus": "ArchiveSuccess",
            "PaymentStatus": "NotPaid",
            "DeliveryAddress": {
                "ZipCode": "634041",
                "AddressLine1": "улица Тверская, 81",
                "AddressLine2": "",
                "Description": "",
                "City": "Томск",
                "Country": "Россия",
                "State": "",
                "FullName": "Виктор Михалев",
                "Phone": "79528007248"
            },
            "Shipping": {
                "Id": 40845,
                "Title": "Курьер компании - Томск",
                "Phone": null,
                "Email": null,
                "Type": "Courier"
            },
            "CommentsCount": 0,
            "DownloadLink": "https://cdn.pixlpark.com/data2/orders/1164/2882993/2021-07-22_1164_2882993_2b41.zip",
            "PreviewImageUrl": "https://cdn.pixlpark.com/data2/orders/1164/2882993/preview.jpg",
            "Price": 1800.0,
            "DiscountPrice": 180.0,
            "DeliveryPrice": 390.0,
            "TotalPrice": 2010.0,
            "PaidPrice": 0.0,
            "UserId": 815824,
            "UserCompanyAccountId": 2625,
            "DiscountTitle": "10%",
            "DateCreated": "Date(1626974880000)",
            "DateModified": "Date(1626974880000)",
            "DatePaid": null,
            "DateReady": null,
            "LastDownloadedPaymentDocument": null,
            "PaymentSystemUniqueId": null,
            "GoogleClientId": null,
            "ContractorOrderNumber": null,
            "ContractorOrderTotalPrice": 0.0
        },
        {
            "Id": "2881068",
            "PhotolabId": 1164,
            "CustomId": "2881068",
            "SourceOrderId": null,
            "ManagerId": null,
            "AssignedToId": null,
            "Title": "Футболки (Женская белая); Размер и количество (XS))",
            "TrackingUrl": "",
            "TrackingNumber": null,
            "Status": "NotProcessed",
            "RenderStatus": "ArchiveSuccess",
            "PaymentStatus": "NotPaid",
            "DeliveryAddress": {
                "ZipCode": "634021",
                "AddressLine1": "пр. Фрунзе 117а, офис 605",
                "AddressLine2": "",
                "Description": "",
                "City": "Томск",
                "Country": "Россия",
                "State": "Томская область",
                "FullName": "Груст Туст",
                "Phone": "+7 (533) 412 31-23"
            },
            "Shipping": {
                "Id": 8100,
                "Title": "Офис компании на Фрунзе",
                "Phone": "+7(495)545-46-91",
                "Email": null,
                "Type": "Point"
            },
            "CommentsCount": 0,
            "DownloadLink": "https://s3-eu-west-1.amazonaws.com/pxl-s/orders/1164/2881068/2021-07-21_1164_2881068_a108.zip",
            "PreviewImageUrl": "https://cdn.pixlpark.com/data2/orders/1164/2881068/preview.jpg",
            "Price": 1800.0,
            "DiscountPrice": 180.0,
            "DeliveryPrice": 0.0,
            "TotalPrice": 1620.0,
            "PaidPrice": 0.0,
            "UserId": 106162643,
            "UserCompanyAccountId": 29081,
            "DiscountTitle": "10%",
            "DateCreated": "Date(1626865800000)",
            "DateModified": "Date(1626865800000)",
            "DatePaid": null,
            "DateReady": null,
            "LastDownloadedPaymentDocument": null,
            "PaymentSystemUniqueId": null,
            "GoogleClientId": "1255670613.1626347755",
            "ContractorOrderNumber": null,
            "ContractorOrderTotalPrice": 0.0
        },
        {
            "Id": "2880747",
            "PhotolabId": 1164,
            "CustomId": "2880747",
            "SourceOrderId": null,
            "ManagerId": null,
            "AssignedToId": null,
            "Title": "Магниты (6x9 см)",
            "TrackingUrl": "",
            "TrackingNumber": null,
            "Status": "NotProcessed",
            "RenderStatus": "ArchiveSuccess",
            "PaymentStatus": "NotPaid",
            "DeliveryAddress": {
                "ZipCode": "",
                "AddressLine1": "ул. Октябрьская, 104",
                "AddressLine2": "",
                "Description": "",
                "City": "Рубцовск",
                "Country": "Россия",
                "State": "Алтайский край",
                "FullName": "Груст Туст",
                "Phone": "75334123123"
            },
            "Shipping": {
                "Id": 8105,
                "Title": "СДЕК (до склада)",
                "Phone": null,
                "Email": null,
                "Type": "Mail"
            },
            "CommentsCount": 0,
            "DownloadLink": "https://s3-eu-west-1.amazonaws.com/pxl-s/orders/1164/2880747/2021-07-21_1164_2880747_7470.zip",
            "PreviewImageUrl": "https://cdn.pixlpark.com/data2/orders/1164/2880747/preview.jpg",
            "Price": 180.0,
            "DiscountPrice": 18.0,
            "DeliveryPrice": 589.62,
            "TotalPrice": 751.62,
            "PaidPrice": 0.0,
            "UserId": 106162643,
            "UserCompanyAccountId": 29081,
            "DiscountTitle": "10% бонусов на первый заказ,10%",
            "DateCreated": "Date(1626856020000)",
            "DateModified": "Date(1626856020000)",
            "DatePaid": null,
            "DateReady": null,
            "LastDownloadedPaymentDocument": null,
            "PaymentSystemUniqueId": null,
            "GoogleClientId": "1255670613.1626347755",
            "ContractorOrderNumber": null,
            "ContractorOrderTotalPrice": 0.0
        },
        {
            "Id": "2880644",
            "PhotolabId": 1164,
            "CustomId": "2880644",
            "SourceOrderId": null,
            "ManagerId": null,
            "AssignedToId": null,
            "Title": "Фотокниги в мягкой обложке (15x20 см)",
            "TrackingUrl": "",
            "TrackingNumber": null,
            "Status": "NotProcessed",
            "RenderStatus": "ArchiveSuccess",
            "PaymentStatus": "NotPaid",
            "DeliveryAddress": {
                "ZipCode": "",
                "AddressLine1": "пр.Фрунзе, 86",
                "AddressLine2": "",
                "Description": "Пересечение улиц Красноармейская и Фрунзе, вход ТЦ Европа со стороны ул. Красноармейская. Остановка общественного транспорта ЦОТ",
                "City": "Томск",
                "Country": "Россия",
                "State": "Томская обл.",
                "FullName": "Виктор Вахитов",
                "Phone": "79069475081"
            },
            "Shipping": {
                "Id": 8105,
                "Title": "СДЕК (до склада)",
                "Phone": null,
                "Email": null,
                "Type": "Mail"
            },
            "CommentsCount": 0,
            "DownloadLink": "https://s3-eu-west-1.amazonaws.com/pxl-s/orders/1164/2880644/2021-07-21_1164_2880644_6c34.zip",
            "PreviewImageUrl": "https://cdn.pixlpark.com/data2/orders/1164/2880644/preview.jpg",
            "Price": 1800.0,
            "DiscountPrice": 180.0,
            "DeliveryPrice": 508.2,
            "TotalPrice": 2128.2,
            "PaidPrice": 0.0,
            "UserId": 69985494,
            "UserCompanyAccountId": null,
            "DiscountTitle": "10%",
            "DateCreated": "Date(1626852600000)",
            "DateModified": "Date(1626852600000)",
            "DatePaid": null,
            "DateReady": null,
            "LastDownloadedPaymentDocument": null,
            "PaymentSystemUniqueId": null,
            "GoogleClientId": "656183119.1623311908",
            "ContractorOrderNumber": null,
            "ContractorOrderTotalPrice": 0.0
        },
        {
            "Id": "2879567",
            "PhotolabId": 1164,
            "CustomId": "2879567",
            "SourceOrderId": null,
            "ManagerId": null,
            "AssignedToId": null,
            "Title": "Визитки (Цифровая односторонняя, горизонтальная, 90x50 мм); Ламинация (Без ламинации), Тип бумаги (Матовая), Срок исполнения (3 рабочих дня))",
            "TrackingUrl": "",
            "TrackingNumber": null,
            "Status": "NotProcessed",
            "RenderStatus": "ArchiveSuccess",
            "PaymentStatus": "NotPaid",
            "DeliveryAddress": {
                "ZipCode": "",
                "AddressLine1": "пр-т. Ленина, 88",
                "AddressLine2": "",
                "Description": "",
                "City": "Орск",
                "Country": "Россия",
                "State": "Оренбургская обл.",
                "FullName": "Рубэн Гигия",
                "Phone": "79001111112"
            },
            "Shipping": {
                "Id": 8105,
                "Title": "СДЕК (до склада)",
                "Phone": null,
                "Email": null,
                "Type": "Mail"
            },
            "CommentsCount": 0,
            "DownloadLink": "https://s3-eu-west-1.amazonaws.com/pxl-s/orders/1164/2879567/2021-07-20_1164_2879567_6df6.zip",
            "PreviewImageUrl": "https://cdn.pixlpark.com/data2/orders/1164/2879567/preview.jpg",
            "Price": 7650.0,
            "DiscountPrice": 765.0,
            "DeliveryPrice": 824.86,
            "TotalPrice": 7709.86,
            "PaidPrice": 0.0,
            "UserId": 106074672,
            "UserCompanyAccountId": null,
            "DiscountTitle": "10%",
            "DateCreated": "Date(1626776820000)",
            "DateModified": "Date(1626776820000)",
            "DatePaid": null,
            "DateReady": null,
            "LastDownloadedPaymentDocument": null,
            "PaymentSystemUniqueId": null,
            "GoogleClientId": "1255670613.1626347755",
            "ContractorOrderNumber": null,
            "ContractorOrderTotalPrice": 0.0
        },
        {
            "Id": "2879433",
            "PhotolabId": 1164,
            "CustomId": "2879433",
            "SourceOrderId": 2879431,
            "ManagerId": null,
            "AssignedToId": null,
            "Title": "Фотографии (10x15 см) [без полей]",
            "TrackingUrl": "",
            "TrackingNumber": null,
            "Status": "NotProcessed",
            "RenderStatus": "ArchiveSuccess",
            "PaymentStatus": "NotPaid",
            "DeliveryAddress": {
                "ZipCode": "634028",
                "AddressLine1": "ул. Карпова, дом 8, кв. 73",
                "AddressLine2": "",
                "Description": "",
                "City": "Томск",
                "Country": "",
                "State": "",
                "FullName": "Admin Семену",
                "Phone": ""
            },
            "Shipping": {
                "Id": 8104,
                "Title": "Курьер по Томскому району - до 50 км от города",
                "Phone": null,
                "Email": null,
                "Type": "Courier"
            },
            "CommentsCount": 0,
            "DownloadLink": "https://s3-eu-west-1.amazonaws.com/pxl-s/orders/5351/2879431/2021-07-20_1164_2879431_aad0.zip",
            "PreviewImageUrl": "https://cdn.pixlpark.com/data2/orders/1164/2879433/preview.jpg",
            "Price": 648.0,
            "DiscountPrice": 64.8,
            "DeliveryPrice": 0.0,
            "TotalPrice": 583.2,
            "PaidPrice": 0.0,
            "UserId": 106088275,
            "UserCompanyAccountId": 4619,
            "DiscountTitle": "10%",
            "DateCreated": "Date(1626772560000)",
            "DateModified": "Date(1626774300000)",
            "DatePaid": null,
            "DateReady": null,
            "LastDownloadedPaymentDocument": null,
            "PaymentSystemUniqueId": null,
            "GoogleClientId": null,
            "ContractorOrderNumber": "2879431",
            "ContractorOrderTotalPrice": 115.60
        },
        {
            "Id": "2879295",
            "PhotolabId": 1164,
            "CustomId": "2879295",
            "SourceOrderId": null,
            "ManagerId": null,
            "AssignedToId": null,
            "Title": "Фотообои (Бумажные) [292 x 130 см]; Корректировка изображения (Цветокоррекция), Фактура фотообоев (Волокно)), Фотографии (10x15 см) [без полей]; Срок исполнения (24 часа), Тип бумаги (Шелк)) и еще 6 шт.",
            "TrackingUrl": "",
            "TrackingNumber": null,
            "Status": "NotProcessed",
            "RenderStatus": "ArchiveSuccess",
            "PaymentStatus": "NotPaid",
            "DeliveryAddress": {
                "ZipCode": "",
                "AddressLine1": "пр-т. Ленина, 88",
                "AddressLine2": "",
                "Description": "",
                "City": "Орск",
                "Country": "Россия",
                "State": "Оренбургская обл.",
                "FullName": "Рубэн Гигия",
                "Phone": "79001111112"
            },
            "Shipping": {
                "Id": 8105,
                "Title": "СДЕК (до склада)",
                "Phone": null,
                "Email": null,
                "Type": "Mail"
            },
            "CommentsCount": 0,
            "DownloadLink": "https://s3-eu-west-1.amazonaws.com/pxl-s/orders/1164/2879295/2021-07-20_1164_2879295_5654.zip",
            "PreviewImageUrl": "https://cdn.pixlpark.com/data2/orders/1164/2879295/preview.jpg",
            "Price": 40315.55,
            "DiscountPrice": 4031.555,
            "DeliveryPrice": 1118.84,
            "TotalPrice": 37402.84,
            "PaidPrice": 0.0,
            "UserId": 106074672,
            "UserCompanyAccountId": null,
            "DiscountTitle": "10%",
            "DateCreated": "Date(1626767640000)",
            "DateModified": "Date(1626767640000)",
            "DatePaid": null,
            "DateReady": null,
            "LastDownloadedPaymentDocument": null,
            "PaymentSystemUniqueId": null,
            "GoogleClientId": "1255670613.1626347755",
            "ContractorOrderNumber": null,
            "ContractorOrderTotalPrice": 0.0
        },
        {
            "Id": "2879155",
            "PhotolabId": 1164,
            "CustomId": "2879155",
            "SourceOrderId": null,
            "ManagerId": null,
            "AssignedToId": null,
            "Title": "произвольный заказ",
            "TrackingUrl": "",
            "TrackingNumber": null,
            "Status": "NotProcessed",
            "RenderStatus": "ArchiveSuccess",
            "PaymentStatus": "NotPaid",
            "DeliveryAddress": null,
            "Shipping": null,
            "CommentsCount": 0,
            "DownloadLink": "https://s3-eu-west-1.amazonaws.com/pxl-s/orders/1164/2879155/2021-07-20_1164_2879155_76fc.zip",
            "PreviewImageUrl": "https://cdn.pixlpark.com/data2/orders/1164/2879155/preview.jpg",
            "Price": 0.0,
            "DiscountPrice": 0.0,
            "DeliveryPrice": 0.0,
            "TotalPrice": 0.0,
            "PaidPrice": 0.0,
            "UserId": 106074672,
            "UserCompanyAccountId": null,
            "DiscountTitle": "10%",
            "DateCreated": "Date(1626760500000)",
            "DateModified": "Date(1626760500000)",
            "DatePaid": null,
            "DateReady": null,
            "LastDownloadedPaymentDocument": null,
            "PaymentSystemUniqueId": null,
            "GoogleClientId": "1255670613.1626347755",
            "ContractorOrderNumber": null,
            "ContractorOrderTotalPrice": 0.0
        },
        {
            "Id": "2879148",
            "PhotolabId": 1164,
            "CustomId": "2879148",
            "SourceOrderId": null,
            "ManagerId": null,
            "AssignedToId": null,
            "Title": "Визитки (Цифровая односторонняя, горизонтальная, 90x50 мм); Тип бумаги (Матовая), Ламинация (Матовая), Срок исполнения (2 часа))",
            "TrackingUrl": "",
            "TrackingNumber": null,
            "Status": "NotProcessed",
            "RenderStatus": "ArchiveSuccess",
            "PaymentStatus": "NotPaid",
            "DeliveryAddress": {
                "ZipCode": "634029",
                "AddressLine1": "ул. Белинского, 18",
                "AddressLine2": "",
                "Description": "",
                "City": "Томск",
                "Country": "",
                "State": "",
                "FullName": "Рубэн Гигия",
                "Phone": "+7 (900) 111 11-12"
            },
            "Shipping": {
                "Id": 8106,
                "Title": "Офис компании на Железнодорожной",
                "Phone": "0",
                "Email": null,
                "Type": "Point"
            },
            "CommentsCount": 0,
            "DownloadLink": "https://s3-eu-west-1.amazonaws.com/pxl-s/orders/1164/2879148/2021-07-20_1164_2879148_18eb.zip",
            "PreviewImageUrl": "https://cdn.pixlpark.com/data2/orders/1164/2879148/preview.jpg",
            "Price": 4400.0,
            "DiscountPrice": 440.0,
            "DeliveryPrice": 0.0,
            "TotalPrice": 3960.0,
            "PaidPrice": 0.0,
            "UserId": 106074672,
            "UserCompanyAccountId": null,
            "DiscountTitle": "10% бонусов на первый заказ,10%",
            "DateCreated": "Date(1626759780000)",
            "DateModified": "Date(1626759780000)",
            "DatePaid": null,
            "DateReady": null,
            "LastDownloadedPaymentDocument": null,
            "PaymentSystemUniqueId": null,
            "GoogleClientId": "1255670613.1626347755",
            "ContractorOrderNumber": null,
            "ContractorOrderTotalPrice": 0.0
        },
        {
            "Id": "2879132",
            "PhotolabId": 1164,
            "CustomId": "2879132",
            "SourceOrderId": null,
            "ManagerId": null,
            "AssignedToId": null,
            "Title": "Флаеры (Цифровая односторонняя, Евро); Дизайн (Стандартный))",
            "TrackingUrl": "",
            "TrackingNumber": null,
            "Status": "NotProcessed",
            "RenderStatus": "ArchiveSuccess",
            "PaymentStatus": "NotPaid",
            "DeliveryAddress": {
                "ZipCode": "5435451",
                "AddressLine1": "мавы 423, 123",
                "AddressLine2": "",
                "Description": "мва",
                "City": "Томск",
                "Country": "Россия",
                "State": "",
                "FullName": "шмоцул двал",
                "Phone": "79795834712"
            },
            "Shipping": {
                "Id": 40845,
                "Title": "Курьер компании - Томск",
                "Phone": null,
                "Email": null,
                "Type": "Courier"
            },
            "CommentsCount": 1,
            "DownloadLink": "https://s3-eu-west-1.amazonaws.com/pxl-s/orders/1164/2879132/2021-07-20_1164_2879132_6421.zip",
            "PreviewImageUrl": "https://cdn.pixlpark.com/data2/orders/1164/2879132/preview.jpg",
            "Price": 1125.0,
            "DiscountPrice": 112.5,
            "DeliveryPrice": 390.0,
            "TotalPrice": 1502.5,
            "PaidPrice": 0.0,
            "UserId": 106073364,
            "UserCompanyAccountId": null,
            "DiscountTitle": "10% бонусов на первый заказ,10%",
            "DateCreated": "Date(1626758820000)",
            "DateModified": "Date(1626758880000)",
            "DatePaid": null,
            "DateReady": null,
            "LastDownloadedPaymentDocument": null,
            "PaymentSystemUniqueId": "Cash",
            "GoogleClientId": "1255670613.1626347755",
            "ContractorOrderNumber": null,
            "ContractorOrderTotalPrice": 0.0
        }
    ],
    "ResponseCode": 200
}`;
      const response = JSON.parse(ordersJSON);
      this.orders = response.Result;
      console.log(this.orders[0].DeliveryAddress.Country);
      this.isInvis = true;
    },
  },
};
</script>

<style lang="scss">
.app  {
  
}
.download {
  text-align: center;
}
.content {
}
.item {
}
.item__header {
}
.item__img {
}
.item__statuses {
}
.item__status {
}
.item__paid-status {
}
.item__subheader {
}
.item__delivery {
}
.item__customer-name {
}
.item__customer-phone {
}
.item__address {
}
.item__country {
}
.item__city {
}
.item__street {
}
.item__zip {
}
.item__shipping {
}
.item__shipping-type {
}
.item__shipping-phone {
}
.none {
  display: none;
}
</style>
