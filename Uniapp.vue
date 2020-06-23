//*UNIapp定位(高德地图示例)*/
1.data中定义{
    amapPlugin: null,
    key: '19e7777b7df16b1ec30c4995e81446a2',//密钥
}
2.JS中实现定位
getRegeo() { //获取位置信息
				let _this = this
				console.log('获取定位')
				uni.getSetting({ //查看是否已经授权
					success(res) {
						if (res.authSetting['scope.userLocation']) { //已经授权成功
							_this.amapPlugin.getRegeo({
								success: (data) => { //位置获取成功
									console.log(data)
									// let locationaddress = data[0].name
									console.log(data[0].name)
									_this.locationaddress = data[0].name
									// _this.setloactionaddress(locationaddress)
									let address = data[0].regeocodeData.addressComponent.city
									_this.orgData.type = address
									_this.getOrganizations();
									// _this.activeCity.title = address //当前定位城市
									// _this.setStoreLocation(address) //城市信息设置到store
									uni.hideLoading();
									fail: (error) => {
										uni.showToast({
											title: '位置获取失败',
											icon: 'none',
											duration: 2000
										})
									}
								},
							});
						} else if (!res.authSetting['scope.userLocation']) { //用户未授权前往授权
							uni.authorize({
								scope: 'scope.userLocation',
								success() {
									_this.amapPlugin.getRegeo({
										success: (data) => { //位置获取成功
											console.log(data)
											_this.locationaddress = data[0].name
											let address = data[0].regeocodeData.addressComponent.city
											_this.orgData.type = address
											_this.getOrganizations();
											// _this.activeCity.title = address //当前定位城市
											// _this.setStoreLocation(address) //城市信息设置到store
											uni.hideLoading();
										},
									});
								},
								fail() {
									uni.showModal({
										content: '检测到您没打开获取信息功能权限，是否打开？',
										confirmText: "确认",
										cancelText: '取消',
										success: (res) => {
											console.log(res)
											if (res.confirm) {
												uni.openSetting({
													success: (res) => {
														console.log(res);
													},
													fail: (err) => {
														console.log(err)
													}
												})
											}
										}
									})
								}
							})
						}
					}
				})
			},
/******************************分割线**********************************/

