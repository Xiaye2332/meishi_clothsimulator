<script lang="ts" setup>
import {h, reactive, ref} from "vue";
import 'element-plus/es/components/form/style/css'
import {MagicStick, Refresh, Search, List} from "@element-plus/icons-vue";
import {ElMessage, ElTag} from "element-plus";
import {
  tableData_BoyHat,
  tableData_BoyGlass,
  tableData_BoyCloth,
  tableData_BoyWing,
  tableData_BoySuit,
  tableData_GirlHat,
  tableData_GirlGlass,
  tableData_GirlCloth,
  tableData_GirlWing,
  tableData_GirlSuit,
  tableData_BoyHair,
  tableData_BoyDecoration,
  tableData_BoyEye,
  tableData_GirlHair,
  tableData_GirlDecoration,
  tableData_GirlEye
} from "~/assets/avatar.ts"
import {
  sanjian_list, sanjian_options
} from "~/assets/sanjian.ts";

const baseurl = "https://q.ms.huanlecdn.com/4399/cdn.123u.com/images/1/4/0x"
const baseurl_small = "https://q.ms.huanlecdn.com/4399/cdn.123u.com/images/2/4/0x"
const form = reactive({
  gender: '男',
  hat: '',
  glass: '',
  hair: '',
  decoration: '',
  cloth: '',
  eye: '',
  wing: '',
  suit: '',
})

const yushe = ref(["男", "0", "0", "0", "0", "0", "0", "0", "0"])
const list_change = ref("眼睛")
const table_name = ref([{id: '', name: '请先在左侧选择分类', sex: '', type: 'undefined', hp: '0'}])
const strength_table_name = ref([{id: '', name: '无服装', sex: '', type: 'undefined', hp: '0'}])
const avatar_search = ref("")
const sanjian = ref()
const strength_value = ref(0)
const url = ref('./BoyBaseHead.png')
const url1 = ref('./BoyBaseBody.png')
const url2 = ref('./transparent.png')
const url3 = ref('./transparent.png')
const url4 = ref('./transparent.png')
const url5 = ref('./transparent.png')
const url6 = ref('./transparent.png')
const url7 = ref('./transparent.png')
const url8 = ref('./transparent.png')
const url9 = ref('./transparent.png')

function getImageWidth(url: string) {
  return new Promise((resolve) => {
    const img = new Image();
    img.onload = function () {
      resolve(img.width);
    };
    img.onerror = function () {
      console.log("无法加载图片");
    };
    img.src = url;
  });
}

function Load_Yushe() {
  let temp = yushe.value.toString()
  let temp_list = temp.split(',')
  if (temp_list.length != 9)
    Message_Alert("error", "预设无效，请检查")
  else
    New_Cloth()
}

//衣服渲染方法
function New_Cloth() {
  let temp = yushe.value.toString()
  let temp_list = temp.split(',')
  form.gender = (temp_list[0] == "0") ? "男" : temp_list[0]
  form.glass = (temp_list[1] == "0") ? "" : temp_list[1]
  form.decoration = (temp_list[2] == "0") ? "" : temp_list[2]
  form.cloth = (temp_list[3] == "0") ? "" : temp_list[3]
  form.eye = (temp_list[4] == "0") ? "" : temp_list[4]
  form.hair = (temp_list[5] == "0") ? "" : temp_list[5]
  form.hat = (temp_list[6] == "0") ? "" : temp_list[6]
  form.wing = (temp_list[7] == "0") ? "" : temp_list[7]
  form.suit = (temp_list[8] == "0") ? "" : temp_list[8]
  url.value = (form.gender == "男") ? './BoyBaseHead.png' : './GirlBaseHead.png'
  url1.value = (form.gender == "男") ? './BoyBaseBody.png' : './GirlBaseBody.png'
  let hair1 = document.getElementById("hair1")
  let hair2 = document.getElementById("hair2")
  if (form.gender != "男" && form.gender != "女") {
    Message_Alert("error", "性别选择错误，请检查")
    return
  }
  //查看头发是否大于142，如果是则调整z-index
  getImageWidth((form.hair == "") ? './BoyBaseHair.png' : baseurl + form.hair + ".png")
      .then(width => {
        if (Number(width) > 142) {
          if (hair1)
            hair1.style.zIndex = "14"
          if (hair2)
            hair2.style.zIndex = "20"
        } else {
          if (hair1)
            hair1.style.zIndex = "20"
          if (hair2)
            hair2.style.zIndex = "14"
        }
      });
  //如果不选择套装，则渲染其他
  if (form.suit == "") {
    if (form.gender == "男") {
      url3.value = (form.glass == "") ? './transparent.png' : baseurl + form.glass + ".png"
      url5.value = (form.decoration == "") ? './transparent.png' : baseurl + form.decoration + ".png"
      url6.value = (form.cloth == "") ? './BoyBaseClothes.png' : baseurl + form.cloth + ".png"
      url7.value = (form.eye == "") ? './BoyBaseEye.png' : baseurl + form.eye + ".png"
      url2.value = (form.hat == "") ? './transparent.png' : baseurl + form.hat + ".png"
      url4.value = (form.hair == "") ? './BoyBaseHair.png' : baseurl + form.hair + ".png"
      url8.value = (form.wing == "") ? './transparent.png' : baseurl + form.wing + ".png"
      url9.value = './transparent.png'
    } else {
      url3.value = (form.glass == "") ? './transparent.png' : baseurl + form.glass + ".png"
      url5.value = (form.decoration == "") ? './transparent.png' : baseurl + form.decoration + ".png"
      url6.value = (form.cloth == "") ? './GirlBaseClothes.png' : baseurl + form.cloth + ".png"
      url7.value = (form.eye == "") ? './GirlBaseEye.png' : baseurl + form.eye + ".png"
      url2.value = (form.hat == "") ? './transparent.png' : baseurl + form.hat + ".png"
      url4.value = (form.hair == "") ? './GirlBaseHair.png' : baseurl + form.hair + ".png"
      url8.value = (form.wing == "") ? './transparent.png' : baseurl + form.wing + ".png"
      url9.value = './transparent.png'
    }
  }
  //只需要渲染套装，无需区分男女
  else {
    url2.value = './transparent.png'
    url3.value = './transparent.png'
    url4.value = './transparent.png'
    url5.value = './transparent.png'
    url6.value = './transparent.png'
    url7.value = './transparent.png'
    url8.value = './transparent.png'
    url9.value = (form.suit == "") ? './transparent.png' : baseurl + form.suit + ".png"
  }
  //获取体力
  Get_Strength_Value()
}

const Handle_Change_List = () => {
  column_name.value = columns
  yushe.value = yushe.value.toString().split(',')
  yushe.value[0] = form.gender
  switch (list_change.value) {
    case "眼睛":
      if (form.gender == "男")
        table_name.value = tableData_BoyEye
      else
        table_name.value = tableData_GirlEye
      break
    case "眼镜":
      if (form.gender == "男")
        table_name.value = tableData_BoyGlass
      else
        table_name.value = tableData_GirlGlass
      break
    case "脸饰":
      if (form.gender == "男")
        table_name.value = tableData_BoyDecoration
      else
        table_name.value = tableData_GirlDecoration
      break
    case "翅膀":
      if (form.gender == "男")
        table_name.value = tableData_BoyWing
      else
        table_name.value = tableData_GirlWing
      break
    case "衣服":
      if (form.gender == "男")
        table_name.value = tableData_BoyCloth
      else
        table_name.value = tableData_GirlCloth
      break
    case "帽子":
      if (form.gender == "男")
        table_name.value = tableData_BoyHat
      else
        table_name.value = tableData_GirlHat
      break
    case "头发":
      if (form.gender == "男")
        table_name.value = tableData_BoyHair
      else
        table_name.value = tableData_GirlHair
      break
    case "套装":
      if (form.gender == "男")
        table_name.value = tableData_BoySuit
      else
        table_name.value = tableData_GirlSuit
      break
  }

}
const HandleUse = (data: any) => {
  let type = data.rowData.type
  yushe.value = yushe.value.toString().split(',')
  yushe.value[0] = form.gender
  switch (type) {
    case "眼镜":
      form.glass = data.rowData.id
      yushe.value[1] = data.rowData.id
      break
    case "脸饰":
      form.decoration = data.rowData.id
      yushe.value[2] = data.rowData.id
      break
    case "衣服":
      form.cloth = data.rowData.id
      yushe.value[3] = data.rowData.id
      break
    case "眼睛":
      form.eye = data.rowData.id
      yushe.value[4] = data.rowData.id
      break
    case "头发":
      form.hair = data.rowData.id
      yushe.value[5] = data.rowData.id
      break
    case "帽子":
      form.hat = data.rowData.id
      yushe.value[6] = data.rowData.id
      break
    case "翅膀":
      form.wing = data.rowData.id
      yushe.value[7] = data.rowData.id
      break
    case "套装":
      form.suit = data.rowData.id
      yushe.value[8] = data.rowData.id
      break
  }
  New_Cloth()
}

function HandleRemove(data: any) {
  let type = data.rowData.type
  yushe.value = yushe.value.toString().split(',')
  yushe.value[0] = form.gender
  switch (type) {
    case "眼镜":
      form.glass = data.rowData.id
      yushe.value[1] = "0"
      break
    case "脸饰":
      form.decoration = data.rowData.id
      yushe.value[2] = "0"
      break
    case "衣服":
      form.cloth = data.rowData.id
      yushe.value[3] = "0"
      break
    case "眼睛":
      form.eye = data.rowData.id
      yushe.value[4] = "0"
      break
    case "头发":
      form.hair = data.rowData.id
      yushe.value[5] = "0"
      break
    case "帽子":
      form.hat = data.rowData.id
      yushe.value[6] = "0"
      break
    case "翅膀":
      form.wing = data.rowData.id
      yushe.value[7] = "0"
      break
    case "套装":
      form.suit = data.rowData.id
      yushe.value[8] = "0"
      break
  }
  New_Cloth()
  Check_Selected()
}

function GetSmallPicUrl(data: any) {
  if (data.rowData.id == "" || data.rowData.id == "0" || data.rowData.name == "不穿")
    return './transparent.png'
  return baseurl_small + data.rowData.id.replace(1, "2") + ".png"
}

function HandleImgFail(part: string) {
  let error_part = ""
  switch (part) {
    case "wing":
      url8.value = './transparent.png'
      error_part = "翅膀"
      break
    case "suit":
      url9.value = './transparent.png'
      error_part = "套装"
      break
    case "eye":
      if (form.gender == "男")
        url7.value = './BoyBaseEye.png'
      else
        url7.value = './GirlBaseEye.png'
      error_part = "眼睛"
      break
    case "decoration":
      url5.value = './transparent.png'
      error_part = "脸饰"
      break
    case "cloth":
      if (form.gender == "男")
        url6.value = './BoyBaseClothes.png'
      else
        url6.value = './GirlBaseClothes.png'
      error_part = "衣服"
      break
    case "hair":
      if (form.gender == "男")
        url4.value = './BoyBaseHair.png'
      else
        url4.value = './GirlBaseHair.png'
      error_part = "头发"
      break
    case "hat":
      url2.value = './transparent.png'
      error_part = "帽子"
      break
    case "glass":
      url3.value = './transparent.png'
      error_part = "眼镜"
      break
  }
  Message_Alert("error", "加载" + error_part + "失败，请检查预设是否正确或游戏内是否有此外显")
}

const Message_Alert = (type: string, message: string) => {
  if (type == "error")
    ElMessage.error(message)
  if (type == "success")
    ElMessage({
      message: message,
      type: 'success',
    })
  if (type == "warning")
    ElMessage({
      message: message,
      type: 'warning',
    })
}
const Reset_Yushe = () => {
  yushe.value = ["男", "0", "0", "0", "0", "0", "0", "0", "0"]
  New_Cloth()
  Check_Selected()
}
const columns = [
  // {key: 'id', dataKey: 'id', title: 'ID', width: 100, align: "center",},
  {
    key: "id",
    title: "预览",
    width: 100,
    align: "center",
    cellRenderer: (data: any) => {
      let small_pic_url = GetSmallPicUrl(data);
      return h(
          "div",
          {},
          [
            // 从美食服务器获取缩略图
            h("img", {
              src: small_pic_url,
              width: 45,
            }),
            // 叠加一层透明图片
            h("img", {
              src: "./transparent.png",
              width: 100,
              height: 100,
              style: {
                position: "absolute",
                top: 0,
                left: 0,
                opacity: 0.7,
              },
            }),
          ]
      );
    }

  },
  {key: 'name', dataKey: 'name', title: '名称', width: 150, align: "center"},
  {key: 'sex', dataKey: 'sex', title: '性别', width: 50, align: "center"},
  {
    key: 'type',
    title: '类型',
    width: 80,
    align: "center",
    cellRenderer: (data: any) =>
        h(
            ElTag,
            {
              disableTransitions: true,
            },
            {default: () => data.rowData.type}
        ),
  },
  {
    key: "handle",
    title: "操作",
    width: 100,
    align: "center",
    cellRenderer: (data: any) =>
        h(
            "button",
            {
              onClick: () => HandleUse(data),
              class: "custom-button",
            },
            {default: () => "装备"}
        ),
  }
]


const columns_selected = [
  // {key: 'id', dataKey: 'id', title: 'ID', width: 100, align: "center",},
  {
    key: "id",
    title: "预览",
    width: 100,
    align: "center",
    cellRenderer: (data: any) => {
      let small_pic_url = GetSmallPicUrl(data);
      return h(
          "div",
          {},
          [
            // 从美食服务器获取缩略图
            h("img", {
              src: small_pic_url,
              width: 45,
            }),
            // 叠加一层透明图片
            h("img", {
              src: "./transparent.png",
              width: 100,
              height: 100,
              style: {
                position: "absolute",
                top: 0,
                left: 0,
                opacity: 0.7,
              },
            }),
          ]
      );
    }

  },
  {key: 'name', dataKey: 'name', title: '名称', width: 150, align: "center"},
  {key: 'sex', dataKey: 'sex', title: '性别', width: 50, align: "center"},
  {
    key: 'type',
    title: '类型',
    width: 80,
    align: "center",
    cellRenderer: (data: any) =>
        h(
            ElTag,
            {
              disableTransitions: true,
            },
            {default: () => data.rowData.type}
        ),
  },
  {
    key: "handle",
    title: "操作",
    width: 100,
    align: "center",
    cellRenderer: (data: any) =>
        h(
            "button",
            {
              onClick: () => HandleRemove(data),
              class: "custom-button-selected",
            },
            {default: () => "卸下"}
        ),
  }
]

const strength_columns = [
  // {key: 'id', dataKey: 'id', title: 'ID', width: 100, align: "center",},
  {
    key: "id",
    title: "预览",
    width: 100,
    align: "center",
    cellRenderer: (data: any) => {
      let small_pic_url = GetSmallPicUrl(data);
      return h(
          "div",
          {},
          [
            // 从美食服务器获取缩略图
            h("img", {
              src: small_pic_url,
              width: 45,
            }),
            // 叠加一层透明图片
            h("img", {
              src: "./transparent.png",
              width: 100,
              height: 100,
              style: {
                position: "absolute",
                top: 0,
                left: 0,
                opacity: 0.7,
              },
            }),
          ]
      );
    }

  },
  {key: 'name', dataKey: 'name', title: '名称', width: 150, align: "center"},
  {key: 'hp', dataKey: 'hp', title: '体力', width: 100, align: "center"}
]

function Sanjian_Change() {
  for (let i = 0; i <= 8; i++)
    yushe.value[i] = String(sanjian_list[sanjian.value][i])
  New_Cloth()
}

const column_name = ref(columns)

function Check_Selected() {
  column_name.value = columns_selected
  let temp = yushe.value.toString()
  let idArray = temp.split(',')
  table_name.value = getItemsByIds(idArray)
}

function Get_Strength_Value() {
  let temp = yushe.value.toString()
  let idArray = temp.split(',')
  strength_value.value = getItemsByIds(idArray).reduce((sum, item) => sum + parseInt(item.hp, 10), 0);
  strength_table_name.value = getItemsByIds(idArray)
}

function getItemsByIds(idArray: any) {
  const gender = idArray[0]
  const allTables = [
    tableData_BoyGlass, tableData_BoyDecoration, tableData_BoyCloth, tableData_BoyEye, tableData_BoyHair, tableData_BoyHat, tableData_BoyWing, tableData_BoySuit,
    tableData_GirlGlass, tableData_GirlDecoration, tableData_GirlCloth, tableData_GirlEye, tableData_GirlHair, tableData_GirlHat, tableData_GirlWing, tableData_GirlSuit
  ];
  const result = [];
  for (let i = 1; i < idArray.length; i++) {
    let table = allTables[i - 1 + (gender === '男' ? 0 : 1) * 8];
    let id = idArray[i];
    let item = table.find(item => item.id === id);
    let table_2 = allTables[i - 1 + (gender === '女' ? 0 : 1) * 8];
    let id_2 = idArray[i];
    let item_2 = table_2.find(item_2 => item_2.id === id_2)
    //如果结果id为0，则不显示了
    if (item) {
      if (item.id != "0")
        result.push(item);
    }
    if (item_2) {
      if (item_2.id != "0")
        result.push(item_2);
    }
  }
  return result;
}

//搜索函数
function searchItems(keyword: string) {
  const tableDataList = [
    tableData_BoyGlass,
    tableData_BoyDecoration,
    tableData_BoyCloth,
    tableData_BoyEye,
    tableData_BoyHair,
    tableData_BoyHat,
    tableData_BoyWing,
    tableData_BoySuit,
    tableData_GirlGlass,
    tableData_GirlDecoration,
    tableData_GirlCloth,
    tableData_GirlEye,
    tableData_GirlHair,
    tableData_GirlHat,
    tableData_GirlWing,
    tableData_GirlSuit,
  ];

  const results = [];

  for (const tableData of tableDataList) {
    for (const item of tableData) {
      const match = calculateMatchScore(item.name, keyword);
      if (match > 0) {
        results.push({...item, score: match});
      }
    }
  }

  const threshold = 1; // 设置匹配度阈值，你可以根据需求调整阈值

  return results.filter((item) => item.score >= threshold);
}

// 辅助函数，计算匹配度
function calculateMatchScore(itemName: any, keyword: any) {
  // 这里你可以使用任何合适的字符串匹配算法，比如计算相似度、字符串包含等。
  // 这里简单地使用字符串包含来计算匹配度。
  const lowerCasedKeyword = keyword.toLowerCase();
  const lowerCasedItemName = itemName.toLowerCase();
  return lowerCasedItemName.includes(lowerCasedKeyword)
      ? lowerCasedKeyword.length
      : 0;
}

function SearchAvatar() {
  column_name.value = columns
  table_name.value = searchItems(avatar_search.value)
}

</script>

<template>
  <div style="display: flex; width: 100%">
    <div style="flex: 1">
      <h1 color="$ep-color-primary">服装选择</h1>
      <el-form :model="form" label-width="120px" style="margin-left: 12%">
        <el-form-item label="提示">从右侧服装列表选择你想要的服装</el-form-item>
        <el-form-item label="性别">
          <el-radio-group v-model="form.gender" style="width: 200px" @change="Handle_Change_List">
            <el-radio label="男"/>
            <el-radio label="女"/>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="筛选">
          <el-radio-group v-model="list_change" style="width: 200px" @change="Handle_Change_List">
            <el-radio label="眼睛"/>
            <el-radio label="脸饰"/>
            <el-radio label="衣服"/>
            <el-radio label="眼镜"/>
            <el-radio label="头发"/>
            <el-radio label="帽子"/>
            <el-radio label="翅膀"/>
            <el-radio label="套装"/>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="散件">
          <el-select @change="Sanjian_Change" v-model="sanjian" class="m-2" placeholder="散件" size="default"
                     style="width:200px;margin-left: 0">
            <el-option
                v-for="item in sanjian_options"
                :key="item.value"
                :label="item.label"
                :value="item.value"
            />
          </el-select>
        </el-form-item>
        <el-form-item label="预设">
          <el-input
              style="width: 200px;"
              v-model="yushe"
              :autosize="{ minRows: 3, maxRows: 5 }"
              type="textarea"
              placeholder="预设"
          />
        </el-form-item>
        <el-button style="margin-left: -10%;width: 50px" type="primary" @click="Reset_Yushe()" :icon="Refresh">重置预设
        </el-button>
        <el-button style="width: 50px" type="primary" @click="Load_Yushe();Check_Selected();" :icon="MagicStick">加载预设
        </el-button>
      </el-form>
    </div>
    <div style="flex: 1" id="avatar">
      <h1 style="" color="$ep-color-primary">服装展示</h1>
      <div style="margin-left: -24%">
        <!--翅膀-->
        <img
            id="wing"
            @error="HandleImgFail('wing')"
            style="position: absolute; z-index: 15"
            :src="url8"
        />
        <!--裸头-->
        <img
            id="head"
            style="position: absolute; z-index: 16"
            :src="url"
        />
        <!--脸饰-->
        <img
            id="decoration"
            @error="HandleImgFail('decoration')"
            style="position: absolute; z-index: 19"
            :src="url5"
        />
        <!--眼睛-->
        <img
            id="eye"
            @error="HandleImgFail('eye')"
            style="position: absolute; z-index: 18"
            :src="url7"
        />
        <!--头发1-->
        <img
            style="position: absolute; z-index: 20;clip:rect(0px,142px,184px,0px)"
            id="hair1"
            @error="HandleImgFail('hair')"
            :src="url4"
        />
        <!--头发2-->
        <img
            style="position: absolute; z-index: 14;clip:rect(0px,284px,184px,142px);margin-left: -142px"
            id="hair2"
            :src="url4"
        />
        <!--裸身-->
        <img
            id="body"
            style="position: absolute; z-index: 15"
            :src="url1"
        />
        <!--衣服1-->
        <img
            id="cloth1"
            @error="HandleImgFail('cloth')"
            style="position: absolute; z-index: 15;clip:rect(0px,142px,184px,0px)"
            :src="url6"
        />
        <!--衣服2-->
        <img
            id="cloth2"
            style="position: absolute; z-index: 15;clip:rect(0px,284px,184px,142px);margin-left: -142px"
            :src="url6"
        />
        <!--衣服3-->
        <img
            id="cloth3"
            style="position: absolute; z-index: 15;clip:rect(0px,426px,184px,284px);margin-left: -284px"
            :src="url6"
        />
        <!--衣服4-->
        <img
            id="cloth4"
            style="position: absolute; z-index: 15;clip:rect(0px,568px,184px,426px);margin-left: -426px"
            :src="url6"
        />
        <!--帽子-->
        <img
            id="hat"
            @error="HandleImgFail('hat')"
            style="position: absolute; z-index: 22"
            :src="url2"
        />
        <!--眼镜-->
        <img
            id="glass"
            @error="HandleImgFail('glass')"
            style="position: absolute; z-index: 21"
            :src="url3"
        />
        <!--套装-->
        <img
            id="suit"
            @error="HandleImgFail('suit')"
            style="position: absolute; z-index: 50"
            :src="url9"
        />
        <!--透明遮罩-->
        <img
            style="position: absolute; z-index: 100"
            :src="'./transparent.png'"
        />
      </div>
      <div style="margin-top: 220px;">
        <el-popover placement="right" :width="375" trigger="click">
          <template #reference>
            <el-button type="primary" style="width: 120px">当前体力: {{ strength_value }}</el-button>
          </template>
          <el-table-v2
              :columns="strength_columns"
              :data="strength_table_name"
              :width="350"
              :height="300"
              fixed
          />
        </el-popover>
      </div>

    </div>
    <div style="height: 500px;flex: 1">
      <h1 style="margin-left: -10%" color="$ep-color-primary">服装列表</h1>
      <div>
        <el-form style="margin-left: 6%">
          <el-form-item label="">
            <el-input v-model="avatar_search" style="width: 60%;"></el-input>
            <el-button type="primary" style="margin-left: 20px;width: 50px" :icon="Search" @click="SearchAvatar()">
              搜索
            </el-button>
          </el-form-item>
        </el-form>
      </div>
      <div>
        <el-table-v2
            :columns="column_name"
            :data="table_name"
            :width="500"
            :height="500"
            fixed
        />
      </div>
      <div style="margin-top: 22px;">
        <el-button @click="Check_Selected()" :icon="List" type="primary" style="width: 50px;margin-left: -10%">
          已选清单
        </el-button>
      </div>
    </div>
  </div>
</template>

<style>

.custom-button {
  background-color: #3498db; /* 使用明亮的蓝色作为背景色 */
  color: #fff;
  padding: 5px 20px; /* 增加按钮的垂直内边距 */
  border: none;
  border-radius: 5px;
  font-size: 14px; /* 增加按钮文字大小 */
  cursor: pointer;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); /* 添加轻微的阴影效果 */
  transition: background-color 0.3s ease; /* 平滑过渡背景颜色变化 */
}

.custom-button:hover {
  background-color: #2980b9; /* 设置鼠标悬停时的不同背景颜色 */
}

.custom-button-selected {
  background-color: #e74c3c; /* 更亮的红色背景 */
  color: #fff;
  padding: 5px 20px; /* 增加按钮的垂直内边距 */
  border: none;
  border-radius: 5px;
  font-size: 14px; /* 增加按钮文字大小 */
  cursor: pointer;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); /* 添加轻微的阴影效果 */
  transition: background-color 0.3s ease; /* 平滑过渡背景颜色变化 */
}

.custom-button-selected:hover {
  background-color: #c0392b; /* 设置鼠标悬停时的不同背景颜色 */
}

.ep-button + .ep-button {
  margin-left: 0;
  margin: 4px;
}

.cloth {
  display: inline-block;
  vertical-align: middle;
}
</style>