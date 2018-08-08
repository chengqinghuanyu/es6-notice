Symbol为防止命名冲突的

独一无二的一个普通类型
ES5 的对象属性名都是字符串，这容易造成属性名的冲突。比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法（mixin 模式），新方法的名字就有可能与现有方法产生冲突。如果有一种机制，保证每个属性的名字都是独一无二的就好了，这样就从根本上防止属性名的冲突。这就是 ES6 引入Symbol的原因。
1、保证对象能同名属性，防止被改写
2、应该尽量消除魔术字符串，改由含义清晰的变量代替。
const shapeType = {
  triangle: Symbol()
};
function getArea(shape, options) {
  let area = 0;
  switch (shape) {
    case shapeType.triangle:
      area = .5 * options.width * options.height;
      break;
  }
  return area;
}

getArea(shapeType.triangle, { width: 100, height: 100 });
Symbol.for('name')可以命名相同的一个变量
Symbol.keyFor('name')；Symbol.keyFor方法返回一个已登记的 Symbol 类型值的key。需要注意的是，Symbol.for为 Symbol 值登记的名字，是全局环境的，可以在不同的 iframe 或 service worker 中取到同一个值。
Symbol还有其他11个内置方法，用的不太多。暂时不考虑